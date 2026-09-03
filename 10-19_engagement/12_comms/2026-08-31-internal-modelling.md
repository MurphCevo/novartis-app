# Internal Discovery Modelling

> **TL;DR** - Internal Cevo working session where Paul (BA) walked the team through Diganth's proposed target architecture for the Novartis Autopilot POV, in plain English, to surface open questions for the client.
> - **Scope boundary:** Cevo starts at Snowflake (structured) and S3 (unstructured). Everything upstream (iHub/ETL, SFTP, source extraction) is out of scope. Ongoing/incremental ingestion and schema changes are also out for the POV - initial one-off load only.
> - **Unstructured flow:** Novartis lands files in S3 - Step Functions/Lambda generate metadata (DynamoDB) and convert docs to Markdown - Bedrock KB embeds into hot (OpenSearch Serverless) / cold (S3 Vectors) tiers. Retrieval: intent detection (Haiku) - partition router - parallel BM25 + KNN - Rerank 3.5 - Sonnet synthesis.
> - **Structured flow:** spun-up sub-agent does text-to-SQL against Snowflake. Connection approach is open - Lambda + PrivateLink vs JDBC (ALB + ECS) vs Snowflake Cortex/MCP. Cortex-as-primary-agent ruled out (governance requires agents registered in AWS agentcore).
> - **Trust is the core risk:** an AI that fails silently could collapse adoption. Mitigations: golden dataset (10 users x 10 days), weekly LLM-as-judge evaluation, thumbs up/down feedback loop.
> - **Key open items for Novartis:** how unstructured data reaches S3 (big assumption), whether metadata already exists, Snowflake connection method, semantic-layer ownership, POV user/query numbers and success criteria.
> - **Working decisions:** DEC-001 (Snowflake+S3 boundary), DEC-002 (no incremental ingestion in POV), DEC-003 (no Cortex as primary agent), DEC-004 (data quality is Novartis's contractual responsibility).

**Date:** 2026-08-31
**Type:** Internal Cevo working session (architecture modelling / discovery prep) - not a client meeting
**Duration:** ~68 min across two recordings (56m + 12m)
**Attendees:** Paul Murphy (BA), Diganth Sanghvi (AI/solution lead), Domenico Campagnolo (Dom - infra/data), Ian Ng (delivery lead), Meiyappan Chidambaram (Mei), Ken Lawrie. Mehul referenced (Snowflake, not present).

> Draft working notes. Summary synthesised from the raw transcript preserved below. Most architecture points are working assumptions pending Novartis confirmation, not settled decisions.

---

## Purpose

Paul (BA) walked through Diganth's proposed target architecture for the Novartis Autopilot POV to build a shared, plain-English understanding and surface the open questions to put to the client. The session covered how data gets in, how it is retrieved for both unstructured and structured queries, and how quality and trust are handled.

---

## Summary

### 1. Data sources and boundary
- Novartis has ~9 data sources feeding into an ETL pipeline (referred to as "iHub") that lands into Snowflake. Cevo does not own or build iHub.
- The agreed starting boundary for Cevo is **Snowflake (structured) and S3 (unstructured)**. Anything upstream of that (ETL, source extraction, SFTP) is out of Cevo's scope - taking it on would inflate scope beyond what the ramp period allows.
- Novartis already has a dedicated Snowflake instance; Cevo does not need to stand one up.
- Snowflake is treated as an access/query layer over unified data; where the data physically sits underneath is unconfirmed.

### 2. Unstructured data ingestion and indexing
- Assumption: all unstructured content (PDFs, PowerPoints, Word docs) will be made available in S3 by Novartis. How it lands there is Novartis's responsibility.
- On landing, AWS Step Functions + Lambda generate metadata per file (file name, contents, owning department, document type) and store it in DynamoDB.
- Lambda also converts PDFs/Word files to Markdown to save tokens - proposed via an open-source library, which needs Novartis security sign-off (pharma may reject open source).
- Bedrock Data Automation used for parsing/loading. Bedrock Knowledge Base handles embedding (e.g. Cohere model) into the vector stores.
- **Hot/cold tiering:** frequently queried files go to OpenSearch Serverless (hot, low latency); the rest go to S3 Vectors (cold, cheaper). Split to be worked out with the client and tuned over time via observability (promote/demote files based on actual usage).

### 3. Unstructured retrieval flow
- User query hits API Gateway (assumed web/chat UI - UI ownership is an open question, no UI skillset on the team yet).
- agentcore memory provides per-user short-term (session/episodic) and long-term memory.
- A lightweight Haiku LLM does intent detection and prompt rewrite (compensates for poor user prompting).
- Partition router uses DynamoDB metadata to filter down to relevant files/partitions and decide OpenSearch vs S3 Vectors.
- Parallel retrieval: BM25 (keyword) + KNN (semantic) run together, then Rerank 3.5 re-ranks the chunks. Sonnet synthesises the final human-readable answer.

### 4. Structured retrieval flow
- Runs as a spun-up **sub-agent** from the main agent loop; the same orchestrating agent can split a query across structured and unstructured paths and merge the results.
- Text-to-SQL (Haiku or Sonnet, to be decided by experiment). agentcore code interpreter validates query syntax.
- Only the semantic-layer **schema** is ingested from the structured side (so the LLM knows which tables to query) - assumed static for the POV.
- Two candidate connection approaches (see Decisions/Open items):
  1. **Lambda + AWS PrivateLink** to run plain SQL on the Snowflake warehouse (keeps all logic on Cevo/AWS side).
  2. **JDBC via a scalable compute layer** (ALB + ECS tasks) instead of Lambda, for better scaling and parallel queries.
  3. **Snowflake Cortex Agent via Snowflake MCP into Bedrock** - delegates query/schema understanding to Snowflake, but loses control and conflicts with the governance requirement to register agents in AWS.

### 5. Caching
- Amazon MemoryDB (vs ElastiCache - to be compared on cost) caches answers so repeat queries skip retrieval. Cheaper than repeated token cost over time. Caching TTL configurable to Novartis policy (e.g. 24h).

### 6. Data quality and trust
- Cannot assume clean source data (e.g. a file overwritten with mismatched content vs its metadata). Data quality is proposed as a **Novartis responsibility, captured in the contract**.
- Multimodal ingestion is required (images needed for marketing outputs), so cannot simply restrict to text. Irrelevant content is filtered out at retrieval via cosine similarity, but quality still matters for correct answers.
- **Trust is the core risk:** the manual process is trusted today; an AI that "fails silently" could erode trust and collapse adoption. Mitigations proposed:
  - Continuous evaluation: 10 users over 10 days to build a golden dataset of "what good looks like".
  - LLM-as-judge weekly evaluation over the corpus, producing a report to flag regressions and trigger troubleshooting.
  - Thumbs up/down feedback loop (as done on the ECH engagement) feeding the judge.
  - Note: team lacks healthcare domain expertise, so judging answer correctness is itself hard - reinforces need for client feedback.

### 7. Scope, scale and cost
- Ongoing/incremental ingestion (new or updated documents after initial load) and schema-change handling are **out of scope for the POV** - initial one-off ingestion only.
- Scale: think in queries, not users - one user query can fan out into many Snowflake queries. Need Novartis input on POV user numbers and query nature to size the architecture.
- Access is restricted to a defined data domain/subsection of Snowflake; out-of-domain queries return null (also controls Snowflake cost).
- A cost model was built on Novartis's dummy figures and returned to them (in SharePoint). Token volumes came from Novartis, not Cevo. Caveat communicated: testing likely costs more than production.

---

## Decisions (working)

| ID | Decision | Status |
|---|---|---|
| DEC-001 | Cevo's scope starts at Snowflake (structured) and S3 (unstructured); everything upstream (iHub/ETL) is out of scope | Agreed (team), pending client confirmation |
| DEC-002 | Ongoing/incremental ingestion and schema-change handling are out of scope for the POV | Agreed (team) |
| DEC-003 | Cannot use Snowflake Cortex Agent as the primary agent - governance requires agents registered in AWS agentcore | Accepted, with Cortex-via-MCP retained as a documented alternative |
| DEC-004 | Data quality to be treated as Novartis's responsibility and reflected in the contract | Proposed |

> These IDs match the central Decision Record ([21.01f](../../20-29_problem/21_context/21.01f-decision-record.md)), where each decision is recorded in full. This note is the working capture; the register is authoritative.

---

## Assumptions to validate

| ID | Assumption | Impact if wrong |
|---|---|---|
| ASM-015 | All structured data reaches Snowflake via iHub ETL | Data availability / boundary |
| ASM-009 | All unstructured data (PDF, PPT, Word) is made available in S3 by Novartis | Core to ingestion pipeline - large purple assumption |
| ASM-010 | ~200 GB of unstructured data in total | Tiering and cost design |
| ASM-011 | Snowflake schema stays static for the POV duration | Text-to-SQL accuracy |
| ASM-012 | There is a chat/web UI; UI build ownership unresolved | Delivery - no UI skillset on team |
| ASM-013 | Novartis can indicate which files are most frequently used (hot vs cold) | Tiering accuracy; tunable via observability |

> IDs match the central Assumptions Register ([21.01h](../../20-29_problem/21_context/21.01h-assumptions-register.md)), which holds the full validation detail. ASM-009 to ASM-013 were promoted there from this session; ASM-015 (iHub ETL boundary premise) is added to the register alongside them.

---

## Open questions for Novartis

| ID | Question |
|---|---|
| Q-001 | Is iHub a Novartis-built tool or off-the-shelf, and does all structured data actually route through it? |
| Q-002 | Where is the data physically stored beneath Snowflake? |
| Q-003 | Is metadata already captured for unstructured files (e.g. an S3 path + doc-type table)? A metadata link would simplify the pipeline significantly. |
| Q-004 | How will unstructured data (SharePoint, Word, PPT) actually be delivered into S3? |
| Q-005 | Can we use an open-source library to convert PDFs to Markdown, or does security prohibit open source? |
| Q-006 | How do we connect to Snowflake - can they PrivateLink / VPC-endpoint the instance, and will they provide a Snowflake MCP with schema? |
| Q-007 | Are Cevo building the semantic layer, or is Novartis? (Determines whether Mehul or Dom leads.) |
| Q-008 | Who are the POV users, how many, and what is the nature/volume of their queries (for scaling)? |
| Q-009 | What is the success criteria for the POV? What is the benchmark/"golden" campaign? (Client to define.) |
| Q-010 | Is there AWS funding attached to this project? (Sean/Brian question.) |
| Q-011 | Does the connection approach align with what the SOW/RFQ response committed to? |

> IDs match the central Questions Register ([21.01j](../../20-29_problem/21_context/21.01j-questions-register.md)), which holds owners, why-it-matters, and the discovery-workshop question each maps to. This note is the working capture; the register is authoritative.

---

## Actions

| # | Action | Owner |
|---|--------|-------|
| 1 | Transcribe/write up this session (permissions meant not everyone could work off Confluence live) | Paul |
| 2 | Investigate best Snowflake connection option (Cortex/MCP vs JDBC vs Lambda + PrivateLink) and provide an alternate-architecture diagram to attach | Dom |
| 3 | Feed Snowflake questions to Mehul ahead of his talk with Novartis's head of data in the coming weeks | Ian / Dom / Mehul |
| 4 | Finish the evaluation architecture and publish to Confluence (~2 days) | Diganth |
| 5 | Compare MemoryDB vs ElastiCache on cost for caching layer | Diganth / Dom |
| 6 | Forward the cost model sheet for review | Diganth |
| 7 | Prepare data/inputs so Week 1 workshops can run (Diganth not on-site Week 1; Paul + Ken to run) | Diganth |

---

## Raw Transcript - Part 1

Novartis Autopilot - Discovery Modelling-20260831_150433-Meeting Recording
31 August 2026, 05:04am
56m 2s

Paul Murphy started transcription

Meiyappan Chidambaram   0:03
Okay.

Diganth Sanghvi   0:03
is there. So the idea is there are different data sources, right? So this is something concrete we got from their data engineer. So there are multiple data sources, but they have a pipeline, which is an ETL pipeline, I'm assuming, that's connecting all the data sources to Snowflake.

Domenico Campagnolo   0:09
Right.
Okay.

Diganth Sanghvi   0:25
and they can run it to give us the data in Snowflake if it's not available. And they call it iHub or something like that, if I'm not wrong.

Paul Murphy   0:34
Correct, I have.

Diganth Sanghvi   0:36
Yes.

Domenico Campagnolo   0:36
I have.
Okay, but I have is something they made or is a tools.

Paul Murphy   0:45
Don't know yet.

Diganth Sanghvi   0:47
Don't know yet, but I don't think we, if we have to run that, we won't own that. I think our owners should, yeah.

Domenico Campagnolo   0:47
OK.
Yeah, it's just understand what they have and eventually reusing rather than inventing the wheel. And what is the, so are they storing the data on?

Diganth Sanghvi   0:57
Yeah.
Yep.

Domenico Campagnolo   1:06
Snowflakes.
Because Snowflix is like a layer. I don't know if they are sending that out Snowflix clouds or whatever, or they use another backend, like a database, or where is the data stored?

Diganth Sanghvi   1:12
Yep.
Yep.
Yeah.
That's a question mark. So where it's stored in Snowflake is a question mark.

Paul Murphy   1:25
Yeah.

Diganth Sanghvi   1:29
But what I...

Domenico Campagnolo   1:29
Ron.

Diganth Sanghvi   1:32
thought after what Chaitali said, correct me Paul, if I got it wrong or I understood it wrong. So she suggested like everything, every data source that we have, we need to build a semantic layer on top. But then the data engineer corrected saying that we can.

Paul Murphy   1:40
Mhm.
Yeah.

Diganth Sanghvi   1:51
Now you should create a semantic layer for all the tools that you are trying to connect on Snowflake.

Domenico Campagnolo   1:56
Correct.

Diganth Sanghvi   1:57
So that's what I understood. Like we are building, like I'm assuming Snowflake has the same file structure, like database, schema, tables. So I was just thinking, let's start with like, let's call it agent database, and we have different schemas underneath.
and underneath all the tables that this agent cares about. We are not getting giving him access to all the data bills because that's a redundant process and we shouldn't give access, we should limit the access of the agent as much as possible.

Domenico Campagnolo   2:25
Oops.

Paul Murphy   2:26
No.

Domenico Campagnolo   2:31
If you go, we need to understand that.
So there are two concepts. One is storing the data in snowflakes or making the data accessible through snowflakes. I believe we were talking in the second part. Making the data accessible through snowflakes.

Diganth Sanghvi   2:37
Yeah.
Yep.
Ahh.
Yeah, so that's small.

Domenico Campagnolo   2:50
So they have a different source of data and they use Snowflix to unify the access to the data.

Diganth Sanghvi   2:52
Yeah.
Exactly, yeah.

Domenico Campagnolo   3:00
Okay, yeah, it could be.

Paul Murphy   3:01
Is.

Diganth Sanghvi   3:01
The.

Paul Murphy   3:03
It's going to ping Ken.

Diganth Sanghvi   3:03
Yeah.
Yeah.

Paul Murphy   3:11
Okay.

Diganth Sanghvi   3:11
Hey, man, how are you doing?

Meiyappan Chidambaram   3:13
Yeah, pretty good.
Just going through the architecture still.

Diganth Sanghvi   3:18
Yeah, yeah, no, I a lot in that document, so if you feel that I'm talking in circles, please feel free to call out.

Meiyappan Chidambaram   3:29
No, no, all good. Maybe you're talking in AI terms.

Diganth Sanghvi   3:31
Jeff.
No, no, that's the problem. I do a lot in the documents, so just call me out when I'm talking ****.

Paul Murphy   3:36
Yeah.

Meiyappan Chidambaram   3:41
All good.

Paul Murphy   3:42
We might, let's get started. So what I've got in the centre here is, this is my understanding of...

Diganth Sanghvi   3:44
Yeah.

Domenico Campagnolo   3:44
Yep.

Diganth Sanghvi   3:49
Yep.

Paul Murphy   3:53
what this project's all about. Essentially, if artists want a proof of value, they've got three use cases, and they want to shift their marketing function from manual toil at the moment to intelligent automation using AI. I've got big broader questions.

Diganth Sanghvi   3:55
Ng.
Yup.
Yeah.
Yep.
Yeah.

Paul Murphy   4:13
That I'll pose to Alexis to.
why are we using AI? Not because I'm trying to discourage it, because I really want to get down to the crux of what it is they're trying to do and all their pain points and what have you.

Diganth Sanghvi   4:29
Mhm.

Paul Murphy   4:30
But I think what would be beneficial here is if we start from a real basic statement saying we need data, I need data to put into the system. Cool.

Diganth Sanghvi   4:40
And.
Ken.

Paul Murphy   4:43
Where is it kept? So, at the moment, from what I can understand, we've got nine data sources. Now, if I flip to this particular blue view, these gray...

Diganth Sanghvi   4:45
Mmh.
Mhm.
Mhm.

Paul Murphy   4:57
Boxes here are different systems within Nevadis.

Diganth Sanghvi   4:58
Yup.

Domenico Campagnolo   5:01
Yeah.

Diganth Sanghvi   5:02
Yep.

Paul Murphy   5:03
and they're either going to be and they all may or may not go to this iHub ETL tool.

Diganth Sanghvi   5:09
Yep, that's an assumption. Yep.

Paul Murphy   5:11
That's an assumption, yeah. So let's pop that.

Diganth Sanghvi   5:12
Yeah.

Paul Murphy   5:20
It will.
Potter.
But it's too late to.
Cool.

Diganth Sanghvi   5:28
Yeah.

Paul Murphy   5:30
That's something that we need to get validated. I'll chuck that in here somewhere.

Diganth Sanghvi   5:32
Yeah.
Ken.

Paul Murphy   5:36
Okay. Yeah, sure. Yeah, yeah, yeah.

Diganth Sanghvi   5:37
Can I call out one thing? All structured data goes through UTL.

Paul Murphy   5:45
Yeah, OK. Is that an assumption?

Diganth Sanghvi   5:48
No, like because non-structured data can't go through ETL if I'm not wrong because there's nothing to transfer. Yeah.

Paul Murphy   5:53
Okay, cool.

Domenico Campagnolo   5:55
It depends, it could be for XLS, maybe CSV or whatever, but I believe they are doing some, yeah, maybe unstructured could be the PDF, the build, so whatever, so we assume we are not ETL any unstructured data.

Diganth Sanghvi   6:03
PDF.
Yeah.
Yeah.
Yeah, yeah.
Yeah, that will be available to us in S3. That's another assumption. Like all unstructured data will be available in S3.

Domenico Campagnolo   6:16
Make sense?

Paul Murphy   6:16
Nope.
Ah, okay, so already available in S3.

Diganth Sanghvi   6:28
Yes, they said that, like, like we will make it available in S3, all the PowerPoints, that's what Chitali is going through right now.

Meiyappan Chidambaram   6:33
So.

Domenico Campagnolo   6:37
Did you ask if we already have some sort of metadata pointing to the unstructured data?

Paul Murphy   6:45
Sorry, what's what would?

Diganth Sanghvi   6:46
Oh.

Domenico Campagnolo   6:47
So let's say they are storing this PDF somewhere, but they have the S3 path of this PDF on a table, which a bunch of metadata that say, oh, this is a prescription or this is an invoice or this is something else.

Diganth Sanghvi   7:07
That can be another question here.

Domenico Campagnolo   7:07
If, if there is this link, this simplify a lot of the the.

Diganth Sanghvi   7:13
Yeah.

Ian Ng   7:13
Can we work off confluence instead so we don't have this permissions problem unless all of you guys are in? Because it's all good and well that you're talking about it, but poor Paul has described, right? And I'm sure he'll get it wrong. So that's my ask.

Paul Murphy   7:13
Bob is here.

Meiyappan Chidambaram   7:26
It really depends, yeah, once you do it, I think. I mean, we need to get access to it if we want to, but I think Mehul, yeah, not everyone will.

Ian Ng   7:35
No, at the moment, unless Paul is happy to transcribe after this because he's got the recording. Okay, fine, fair enough. Okay, no worries. Yep, yep.

Paul Murphy   7:41
Yeah, yeah, I can do that, yeah.

Diganth Sanghvi   7:44
Book.

Paul Murphy   7:46
So when you guys say metadata, does that exist at the moment? Do we have to add it or we're not sure? We don't know? No. Good.

Ian Ng   7:55
We don't know, we don't know, we don't know, and we're not sure, yeah.

Domenico Campagnolo   7:56
Yeah.

Diganth Sanghvi   7:56
We don't know. We are not sure.
Yeah, so we can ask the question, are we storing metadata of each and every file of unstructured data?

Paul Murphy   8:00
Excellent.

Ian Ng   8:08
With Ken.

Paul Murphy   8:13
Let me pop that there.
Attach it to there. Okay, so we've got our, let's assume we've got all of our structured data, we've got all of our unstructured data, wherever it's living. How do we get it from A to B?

Diganth Sanghvi   8:18
Yep.
Yeah.
Yeah.
Can you say A to B or where to where you're saying?

Paul Murphy   8:34
How did we get it from where it is?

Diganth Sanghvi   8:36
Mmh.

Paul Murphy   8:37
Into the system.

Diganth Sanghvi   8:39
Okay, if you can scroll to the right where my architecture diagram is.

Paul Murphy   8:44
Beautiful, good.

Diganth Sanghvi   8:44
So, yeah.

Paul Murphy   8:46
Um, I, and I just want to um...
I have read this, I have seen it, and I have like vague ideas of like, or confirmation is what it is, but I'm asking really dumb questions, so it's all obvious. Cool, cool, cool. Sweet. Awesome.

Diganth Sanghvi   8:52
Mhm.
Absolutely, yeah.
Absolutely fine. Happy to answer all your questions. Yeah. Yeah. Yeah. So this is based on the assumption again, like we are going to get structured data in Snowflake and we are going to get unstructured data in S3 starting from there.

Paul Murphy   9:05
Cool.
Mm.
Tom.
Okay, cool. Let me get that.

Diganth Sanghvi   9:16
Yeah, that's an assumption.

Paul Murphy   9:21
In.
snowflake. Do we know if they have a snowflake or we have to? We do.

Diganth Sanghvi   9:26
Yes, we do. They have a snowflake. They said they have a snowflake dedicated. We don't have to do anything about that.

Meiyappan Chidambaram   9:27
Oh yeah, they have a snowflake. They have.

Paul Murphy   9:30
This.

Ian Ng   9:33
Oh.

Meiyappan Chidambaram   9:37
So, is that like assumption? Are they doing it already, or we are planning to do it in this way, Diganth, structure to snowflake and then unstructured test three?

Diganth Sanghvi   9:45
Awesome.

Meiyappan Chidambaram   9:54
Yep.
Okay.

Paul Murphy   10:09
And.

Diganth Sanghvi   10:09
That's the biggest question mark, because they gave us the name of the pipeline or the tool, iHub. So that's my, like, that's a safe assumption right there.

Paul Murphy   10:11
Mm.
Yeah, and so if it's if the...

Diganth Sanghvi   10:19
Yeah.

Paul Murphy   10:24
So is iHub, Snowflake, and S3 together or something different? It's just where they're stored and then iHub either ingests the data, does its thing, spits it out?

Diganth Sanghvi   10:30
No.
Yep.
I have is the pipeline.
I hope is the pipeline I think that runs from all the data sources to Snowflake. I'm assuming it's SFTP at the best. That's how they're doing it. But that again, that's an assumption. Like I'm not even going into IHub. I'm just saying my our starting point should be Snowflake and S3.

Paul Murphy   10:40
Nope.
Nope.
Yeah, nice. Yep.

Diganth Sanghvi   10:59
Anything right to that we shouldn't own because that increases the scope of the project. Yeah.

Meiyappan Chidambaram   11:01
So, because that's where we are taking it, right? That's and then only we are taking it.

Diganth Sanghvi   11:06
Yeah, from the like our starting point is S3 and Snowflake. Anything right to that, it's not our territory and we.

Meiyappan Chidambaram   11:07
Pardon.
Okay.
Okay.

Diganth Sanghvi   11:14
Practically can't deliver it if we anything own on the right.

Meiyappan Chidambaram   11:17
Yeah, got it. Got it. Yeah.

Diganth Sanghvi   11:18
like in this ramp period.

Paul Murphy   11:24
So structured data sits in Snowflake, unstructured data sits in S3. It gets SFTP'd, we're assuming.

Diganth Sanghvi   11:29
Yeah.
No, no, we are not doing the SFTP.

Paul Murphy   11:33
Two.
No, no.

Diganth Sanghvi   11:36
No, we are not doing that. So if we can delete that, yeah, yeah. If we can go to the right.

Paul Murphy   11:37
Swati.

Ian Ng   11:39
I.

Paul Murphy   11:40
Yeah.
Hi.

Diganth Sanghvi   11:45
So let's talk with, let's start with the structured data, how we are going to access it. So my idea is through AWS private link.
and using Lambda and by keeping all the logging and everything inside Secrets Manager. Dom, you tell me if I'm doing something wrong here or this can be improved or I should use an MCP server.

Domenico Campagnolo   12:12
I need to check. So you want to use a lambda to query snowflakes.

Diganth Sanghvi   12:17
Yeah.
Yes, my agent will write query and it will go via Lambda and private link to Snowflake and run the query on the Snowflake warehouse and retrieve the results.

Domenico Campagnolo   12:28
about the league because they live in two different VPNs.

Diganth Sanghvi   12:32
Exactly, yes, yeah, yeah.

Meiyappan Chidambaram   12:33
They are in two different entities.

Domenico Campagnolo   12:33
Okay, so there are also networking. This sounds to me something that probably is already available on Snowflake. Yesterday Mehul shared a link with some sort of...

Diganth Sanghvi   12:36
Yeah.

Ian Ng   12:50
Yesterday was Sunday, man.

Domenico Campagnolo   12:52
Yeah.
What?

Ian Ng   12:54
Yesterday was Sunday, man.

Domenico Campagnolo   12:56
Oh, yes, the last time where it was. Yeah, so I believe Snowflix can offer an interface that avoid us to use AWS Lambda and whatever. So let's do this.

Ian Ng   12:58
Last week, yeah.

Diganth Sanghvi   13:00
OK, yeah.

Meiyappan Chidambaram   13:10
Yeah, they need to create a load balancer.

Diganth Sanghvi   13:10
So how will I link my agent to Snowflake again, Tom? Like, I don't want to use agent inside Snowflake because...
Novartis tracks their agents through agent core framework that they have. Like they want us to register our agents inside AWS and the registry system that they have. We have to work with those designs. So we can't use agents inside Snowflake. We can only use the Snowflake warehouse.
to run the query. So how can I link my agent to that warehouse?

Domenico Campagnolo   13:41
Yeah.
Yes, let me see.

Paul Murphy   13:52
While you're checking that, can I just confirm that the Snowflake and the S3 here are going to sit outside of our AWS account or outside of our structure?

Diganth Sanghvi   13:54
Yeah. Yep.
Yep.
No, no, S3 sits inside, just the snowflake sits outside.

Paul Murphy   14:06
Okay, so is this S3 bucket different to these S3 buckets?

Diganth Sanghvi   14:10
Yeah.
No, that's the same. S3 unstructured. That's the same.

Meiyappan Chidambaram   14:16
Dilva.

Paul Murphy   14:17
Ah, okay. How do we get the unstructured, so SharePoint spreadsheets, Word docs, whatever?

Diganth Sanghvi   14:24
Yeah, that's what they, that's the assumption that we are gonna get all the unstructured data inside S3. That's the biggest assumption here. So how we are gonna get it, that's Novartis has to answer.

Paul Murphy   14:29
Ah.
Ahh.
Okay.

Ian Ng   14:33
But.

Paul Murphy   14:36
Brilliant. No, that's good.

Ian Ng   14:36
I think that.

Diganth Sanghvi   14:37
Yeah, yeah.

Ian Ng   14:38
I think that that requires a pretty big purple assumptions card mate, unless you already have one, so yeah.

Domenico Campagnolo   14:41
Yeah.

Diganth Sanghvi   14:44
Yeah.

Domenico Campagnolo   14:47
So, there are some sort of interface already provided by Snowflix to interact with agents.

Diganth Sanghvi   14:47
Yeah.
Yeah.
Yep.

Domenico Campagnolo   14:58
We need to dig deeper this time. Yeah, I believe every tool is providing some sort of interface. So probably we don't need to remember the wheel. And also, if we use the interface is already implemented, supported, and that's it. So we don't need to

Diganth Sanghvi   15:08
Yeah, of course.
Yeah.

Domenico Campagnolo   15:17
Create a component to connect these two parts.

Diganth Sanghvi   15:20
Yep, I'm happy. Like I was thinking through MCP, but this was the safest way because I have to assume that they will give us access to a Snowflake MCP and that needs to go through another governance standard. But AWS Private Link is the most secured way of doing it. That was my assumption.

Ian Ng   15:20
But from a...

Domenico Campagnolo   15:36
No, I believe the AWS provider link should be there, because this is a sort of networking stuff, but instead to use a lambda, probably we can to query Snowflix, we can use, it's called a Cortex agent. There is also Snowflix Manager MCP server.

Ian Ng   15:37
I think.

Diganth Sanghvi   15:43
Yeah.
Mm.

Domenico Campagnolo   15:55
So, there are a bunch of stuff we can use.

Diganth Sanghvi   15:56
Ohh.
We can't use Cortex agent.

Domenico Campagnolo   16:00
No.

Ian Ng   16:01
Why not?

Diganth Sanghvi   16:02
Because Cortex agent sits inside Snowflake and all the agents needs to be registered on the AWS side of things.

Ian Ng   16:09
Is it worth asking them beforehand? I think we can send them a whole bunch of questions, you know, hey, need clarification on these blah blah blah before we get started.

Diganth Sanghvi   16:11
Chan.

Domenico Campagnolo   16:18
Yeah, I think this could simplify our life rather than going to the universe.

Diganth Sanghvi   16:19
Yep.

Ian Ng   16:22
TS.
Does this solve your... Does this solve that semantic thing that you were talking about last week, Diganth?

Diganth Sanghvi   16:24
Mm.

Domenico Campagnolo   16:27
But yeah, I understand your point. I understand your point because you want to keep the agent on our side rather than using a...

Diganth Sanghvi   16:33
Yeah, yeah, and plus we need to connect the user interface as well, right? So if that Cortex agent is producing the SQL results, but how we'll join it back to the user interface.

Domenico Campagnolo   16:38
TS.
OK, so in this case, if we keep Snowflakes dummy, we use AWS Lambda just to run plain queries and all the logic leads into the our agent. OK, it makes sense to me. Yeah.

Diganth Sanghvi   16:49
Okay.
Yeah. SQL query. Yeah.
Yeah.
Voigt.
Yeah.

Domenico Campagnolo   17:03
It could be right, so AWS Lambda is used just to run a query, so...

Diganth Sanghvi   17:04
Yeah.
Yeah.

Domenico Campagnolo   17:13
I'm thinking. Okay, so I'll do some digging because probably we can use only a JDBC connector even on our side. So we don't need to have that lambda on this slide, whatever.

Diganth Sanghvi   17:19
Yep.
Yeah.
Yep, that's fine. Like as long as we are getting the result back into our AWS ecosystem, because with agent core, we get the gateways as well and the guardrails as well.

Domenico Campagnolo   17:28
But.

Ian Ng   17:28
You still need the private.

Domenico Campagnolo   17:31
Yeah.
Yeah.

Ian Ng   17:37
In the questions, the questions will be, if they have a instance of Snowflake, can you private link it or VPC endpoint or whatever so the agent can have access to it? And then how the agent queries the Snowflake data warehouse, right?

Diganth Sanghvi   17:38
Yeah.
Yeah.
Yeah.

Domenico Campagnolo   17:55
Yeah, Snowflake's become just a data warehouse, so we go there and query.

Ian Ng   17:58
Mm mm.

Diganth Sanghvi   17:59
Yeah, that's fine. Yeah, we can query it as long as it has access to it. The read part. Yeah.

Ian Ng   18:00
Which is?

Domenico Campagnolo   18:00
Yep.

Ian Ng   18:03
Yeah, which is ironic because if you talk to Snowflake aficionados, they can tell you we can do all this in Snowflake already. So, yeah.

Paul Murphy   18:14
So that Snowflakes for structured data, we're going to use AWS to talk to Snowflake via the AWS private link.

Diganth Sanghvi   18:14
Mm.
Yeah.
Yep.
Yep.
Yeah.

Paul Murphy   18:26
How do I, as a business user, tap, tap, tap away at the keyboard to get to that?

Diganth Sanghvi   18:30
Mhm.
Yes, yes, I can answer that. Like, can it take like 15 minutes to explain this architecture from A to Z? Will that help? Yeah? Okay, amazing. Let's start with embedding first, and then we can go with the structured and non-structured.

Paul Murphy   18:36
Sweet.
Yeah, sure, yeah, yeah, go for it. Yeah, yeah, that'd be great, yeah.

Diganth Sanghvi   18:51
part. Yeah.

Paul Murphy   18:51
Just remember, I'm a BA, so you have to go slow.

Diganth Sanghvi   18:55
Absolutely, I will go and if there is any doubts or am I speaking in circles, feel free to reach out and be like, yeah, yeah, yeah.

Paul Murphy   19:00
Cool.
No worries.

Ian Ng   19:04
If you're happy for Paul to interrupt you, yes, eager.

Paul Murphy   19:06
Yeah, where's the embedded layer?

Diganth Sanghvi   19:06
Yes.
bottom ingestion indexing. So let's start with unstructured part and we'll reach to the structured later. Yeah. So first we start with the raw data, right, that we get in S3. Big assumption that we, they run some pipelines, they have something, they land everything in S3. That's the good day of work and we start at S3.

Paul Murphy   19:15
No.
Sweet.
Mhm.
Yep.
Cool.
Yeah.
Swati.

Diganth Sanghvi   19:32
From there, we start creating metadata. Like if they are already recording it, well and good. If not, we sit with them and we help them to understand like as soon as it lands, it goes through AWS step functions, it creates metadata for it. Like for example, what's the file name? What are the file contains?

Paul Murphy   19:49
Ah.

Diganth Sanghvi   19:51
which department this file is for, whether it's regarding HR, marketing, finance, anything. What is this PDF? What includes in this PDF?

Paul Murphy   20:03
Ron.

Meiyappan Chidambaram   20:05
Think better, yeah.

Paul Murphy   20:10
But.
Wright.

Diganth Sanghvi   20:14
their drug test report, if they are standard. So we can pass this through bedrock data automation so we can start loading these and we run it through Lambda and then we store this in DynamoDB. And there is one more functionality of this Lambda.

Paul Murphy   20:18
Yeah.
Wright.

Diganth Sanghvi   20:32
It converts all the PDFs, all Word files to markdowns.

Paul Murphy   20:33
Yep.
Dance.

Diganth Sanghvi   20:41
So, yeah, because it saves us in tokens.

Paul Murphy   20:45
Yeah.

Diganth Sanghvi   20:47
So we are using some open source tools in that, open source libraries, if Novartis allows us. So that's one thing that we want to ask as well. Like, can we use an open source library to convert your PDFs to Markdown?

Paul Murphy   20:57
Oh.
Yes.

Diganth Sanghvi   21:05
Yeah.

Paul Murphy   21:09
So that all feed into a chat that we have with security and the...
What are you all?

Diganth Sanghvi   21:15
Yeah.

Meiyappan Chidambaram   21:16
I don't think that, I don't think so that will be an issue, right? Converting to markdown via open source.

Diganth Sanghvi   21:21
No, there are some people like something as closed as pharma would have issues with using open source technology.

Paul Murphy   21:22
Up there.

Meiyappan Chidambaram   21:29
Okay.

Domenico Campagnolo   21:29
Yeah, they could have some bugs, they could open back doors, you don't know.

Paul Murphy   21:35
Mm.

Diganth Sanghvi   21:35
Yeah, you never know. So it's better to ask them at the start. So the thing is, everything goes to DynamoDB. All the metadata goes to DynamoDB. And I will come around when it's used. Yeah, from S3, we go, if you can scroll to the top, if you can follow the arrow of 14.

Domenico Campagnolo   21:36
Make me some signal.
Yeah.

Paul Murphy   21:40
I.
Mhm.
Yep.
So.
Swati.
Yep.
Ohh, yep, to hit yep, cool.

Diganth Sanghvi   21:56
Yeah, yeah, we can follow the arrow. Now we have to sit with the client and understand what goes into hot tearing, what goes into cold tearing.
Why the distinction, so?
Assumption is, we are getting 200 grids of unstructured data.

Paul Murphy   22:17
Mhm.

Diganth Sanghvi   22:19
Right, we can't shove 200 kids of data into open source serverless, because very expensive.

Paul Murphy   22:19
Yep.

Diganth Sanghvi   22:29
Second, it's not practical.

Paul Murphy   22:32
Mm-hmm.

Diganth Sanghvi   22:33
So we sit with the client and understand which data files they most usually query. What are the, what their teams usually go through in the documents. Like there might be like 100 files, 200 files that they more use. So we can put that in open source serverless.
that is hot storage. In this, latency will be less, and we can work on latency part later. But yeah, that was the hot part. And the rest of it, like something like 1996 study of Panadol on women, like does it impact them or not?

Paul Murphy   22:53
Yep.

Domenico Campagnolo   22:56
Thank you.

Paul Murphy   22:59
Mm.
Cool.
Mhm.
Yeah.

Diganth Sanghvi   23:10
Those are like, it can be still part of it, but it's not the most queried file that can go in S3 vectors. Why? Because it's cheap.

Paul Murphy   23:16
Yeah.
Cool.
Okay, call to you.

Diganth Sanghvi   23:23
So we don't need to shove entire 200 gits to this. So we can put 50 gits to the hot and rest of it to the cold. It can still answer your questions, but it will take some more time. And that's more efficient and cheap.

Paul Murphy   23:26
Cool.
Yep.
Yeah, cool.
Nice.

Diganth Sanghvi   23:38
And what?

Domenico Campagnolo   23:38
So we are creating an extension of the data warehouse in this way, right? Because whatever we are adding, it has a hot cold tiering. It's data.

Diganth Sanghvi   23:49
No, no, no.
That's.
That's unstructured data. It's in vector database. It's not data warehouse. I'm not touched snowflake. Snowflake comes later.

Domenico Campagnolo   24:01
You know, but this is like an extension of the data warehouse, because we are generating some sort of some sort of data that we are storing over there, and we will be used, so should this new data or metadata be part of the actual data warehouse?
Orment.

Diganth Sanghvi   24:23
I will come to that because it's in DynamoDB and that works with a separate part of it. So I will come to that. Yeah. So this knowledge base is basically a bedrock service that helps us to work with how the data will be embedded in OpenSearch and S3 vectors.

Domenico Campagnolo   24:29
Okay.
Yeah.

Diganth Sanghvi   24:42
right? So basically, which model we are using, like cohere model to embed it, because right now it's a PDF, right, in chunks that can't go to the vector store. Basically, each text needs to be represented as a matrix. And this embedding model converts each word, each letters into matrix and draws a map around it.

Domenico Campagnolo   24:44
Okay.

Paul Murphy   24:50
What?

Diganth Sanghvi   25:04
and stores this in a vector data source. That's where knowledge base comes in. It takes those chunks that we have divided now and we have decided these many chunks will set in open search. These many chunks will set at S3 vectors, right? It converts them into matrix and then it puts it.

Paul Murphy   25:04
Mhm.
Mhm.
Oh, I see.

Diganth Sanghvi   25:23
into open source serverless and S3 vectors. That's the entire embedding pipeline.

Paul Murphy   25:23
Yep.
Yep.
Right, so your knowledge base will go, I've got data that I'm going to use frequently, I'm going to go to open search. Data that I'm going to use infrequently, I go to Colti.

Diganth Sanghvi   25:32
Mm.
Yeah.

Meiyappan Chidambaram   25:42
Yes, three.

Diganth Sanghvi   25:43
Yeah, S3, that's it. That's not what it what it does.

Paul Murphy   25:44
Yeah, that's true, yeah, and and those.

Ian Ng   25:47
How does it know? How does it know to separate the two?

Diganth Sanghvi   25:51
We have to sit with the client with this, like water to understand what data will go to hot, what data will go to the cold, because you can't be using 250 gits of data every day. I won't believe you.

Paul Murphy   25:54
At.

Domenico Campagnolo   26:03
What if the client doesn't know what is the most frequent user data?

Paul Murphy   26:04
Deepak.

Diganth Sanghvi   26:08
Then we talk to the team that that will own this.

Domenico Campagnolo   26:13
Yeah, no, most of the time there are clients that doesn't know how to use this data, so they don't assume they know this kind of stuff. They can maybe, yeah, guess it, but...

Diganth Sanghvi   26:19
Uh, that's...

Ian Ng   26:22
I think we have two in this case, right? Yeah.

Diganth Sanghvi   26:23
But right now, it's a map they have to, because they are currently using it to create the marketing docs, so there will be 100 files, at least they have to.

Ian Ng   26:27
Yeah.

Paul Murphy   26:33
Mm.
I think they're going to have a rough idea and it might not be accurate, which I think is fine. But how, yeah, yeah, if it's not, like if it's, they've given us 80% correct, but there's 20% where it's in cold, but it should be in hot or vice versa.

Diganth Sanghvi   26:38
Exactly. Yeah.

Ian Ng   26:39
Yeah, yeah.

Domenico Campagnolo   26:39
Yeah.

Ian Ng   26:41
It.

Diganth Sanghvi   26:41
But we can sit with them, we can workshop it, right? Yeah.
Yeah.
We can find it.

Ian Ng   26:55
We can fine-tune.

Paul Murphy   26:56
Yeah, I'm assuming there's going to be some sort of observability layer there that I'll go, oh, sure.

Diganth Sanghvi   26:57
We can find it. Yeah, yeah.

Domenico Campagnolo   27:01
Yeah.

Diganth Sanghvi   27:01
We will evaluate it, see like which files are getting called again and again, and if it sits in the cold storage, colder tier, we'll bring it back to the hot.

Paul Murphy   27:08
Cool.
Sweet, and vice versa, sweet.

Diganth Sanghvi   27:11
Yeah.
Yeah, vice versa as well. Yeah, that's okay. That's the embedding part. And now your question, like somebody with a business user types a question, how does it retrieve? If you go to the right now, I can tell you so.

Paul Murphy   27:15
Make sense? Okie dokie.
Nope.
Yeah.
Mhm.
Yep.
This for?

Diganth Sanghvi   27:31
Sorry, sorry, left. My bad, I'm dyslexic. Yeah.

Meiyappan Chidambaram   27:32
Yeah.

Paul Murphy   27:33
OK, cool. All good.

Diganth Sanghvi   27:37
So, Amazon API Gateway, I'm just, I'm just imagining a web portal or web app at this stage.

Paul Murphy   27:45
Oh, yep.

Diganth Sanghvi   27:46
So there is a chat interface or something like that. I type, okay, tell me the side effects of neurofin in women in 1996.

Paul Murphy   27:49
Sweat.
Yep.

Diganth Sanghvi   27:56
the study that was done. So what happens is, first of all, you need to log in so we can create a personal profile for you. So where it comes in is memory part. The agent core, memory is easy. Like you have short term, you have long term memory. So it's similar to how you use ChatGPT or Cloud.

Paul Murphy   27:58
Yeah.
Yup.
All right, yep.

Diganth Sanghvi   28:16
right? It knows your coding style. So when it knows how you type, it knows like what you mean when you say things in a certain way. We want to create that for each and every marketing and brand team member. So when they log in, we they start chatting, we start using the memory to store the previous chat sessions and their coding style and the chatting session like.

Paul Murphy   28:21
Mm.
Mhm.
Yep.
Mm.

Diganth Sanghvi   28:36
Okay, this person looks at just structured data. This person just looks at unstructured data, something like that. So that will help us. Yeah, sorry.

Paul Murphy   28:42
Cool.

Ian Ng   28:43
I'm just gonna put.

Paul Murphy   28:44
It's.

Ian Ng   28:45
Okay.

Paul Murphy   28:46
And so that will, oh sorry, I interrupted someone.

Ian Ng   28:48
No, no, no, I'm just going to put my stupid questions in there as well, because, yeah, this is part of what we spoke about the other day as well. Are we going to build a UI for them? No, we're just going to use whatever our request built. So, yeah, I don't even know whether it's in AWS or not. That's the thing. Do you, Paul?

Paul Murphy   28:54
Ng.

Diganth Sanghvi   28:56
Yeah.

Paul Murphy   29:01
Oh.

Diganth Sanghvi   29:01
Yeah, yeah.
Yeah, this is also an assumption here, by the way, the UI part. Not a skilled person in UI.

Ian Ng   29:08
Hmm.

Meiyappan Chidambaram   29:08
So.

Paul Murphy   29:08
No, I did.

Diganth Sanghvi   29:13
Yeah, do we need somebody or not?

Ian Ng   29:14
Yeah.

Meiyappan Chidambaram   29:14
So is that going to be long-term memory or short-term memory over there? I mean it.

Diganth Sanghvi   29:18
Both.

Ian Ng   29:19
Where, where?

Diganth Sanghvi   29:20
But.

Meiyappan Chidambaram   29:21
In the agent core.

Diganth Sanghvi   29:23
Voigt.

Meiyappan Chidambaram   29:24
OK, how do you decide? How do you decide this? This user needs long-term, this needs, or we can enable both of them.

Diganth Sanghvi   29:25
By user.

Paul Murphy   29:25
Right.

Diganth Sanghvi   29:32
We can enable both of them. For session memory is kept anywhere because it is part of the state.py file. Like every time you query it, the previous queries goes with it. So you remember it.

Meiyappan Chidambaram   29:33
Thank you.
Okay.

Ian Ng   29:42
Did you?
Did you say PY file, like a Python file?

Diganth Sanghvi   29:47
Yeah.

Ian Ng   29:48
Okay.

Meiyappan Chidambaram   29:49
Okay.

Diganth Sanghvi   29:49
That's the state.py, that's how it works.

Meiyappan Chidambaram   29:51
The.

Ian Ng   29:52
What the ****? Okay, fair enough.

Diganth Sanghvi   29:53
The.

Paul Murphy   29:54
Don't tell me how to just say magic because if it's just a Python file, then like I go, oh, that's not as exciting as what I was thinking.

Diganth Sanghvi   29:55
Yeah.
It.

Ian Ng   30:01
No, but it's it goes against it goes against some like basic understanding as well, right? You know, but that's how agent call works, so that's fine, you know, just accept it, yeah.

Diganth Sanghvi   30:03
TS.

Paul Murphy   30:08
Hmm.
Well...

Diganth Sanghvi   30:10
Yeah.

Paul Murphy   30:12
I just assume this is all magic.

Ian Ng   30:16
TS.

Diganth Sanghvi   30:16
attention, the short term episodic memory, that's what we call it. So basically whatever you are chatting, so it doesn't forget it. So it's a reinforced part that I usually follow. It send the chat contents with it so that it keeps it in.

Paul Murphy   30:24
Yep.

Meiyappan Chidambaram   30:27
Until I just read the agent, but I didn't know that.

Diganth Sanghvi   30:30
Yeah, yeah. So, and the long-term memory is overall decisions that they made.

Meiyappan Chidambaram   30:33
Thank you.

Paul Murphy   30:34
Mhm.
Ron.

Diganth Sanghvi   30:37
So that's tracked as well, that you can use agent code to figure it out what you want to keep in a long term memory. Like let's say yesterday I asked about this, do you know? And it will go and figure it out in the long term. Yesterday he asked like 10 questions, which one is he talking about? Yeah.

Paul Murphy   30:39
Justin.

Diganth Sanghvi   30:55
For these things, will go in long term. Yeah, again, coming to this, you query something, blah, blah, blah. First, it goes to intent and rewrite. So first thing, a simple haiku LLM will get activated as soon as you prompt. It tries to figure out your intent, like what you are trying to ask.

Paul Murphy   30:59
Mhm.
Mhm.

Diganth Sanghvi   31:14
where it needs to go, and what you're basically trying to do. Because in my experience, most of the people can't prompt an agent correctly, if I'm correct. There is always inefficient prompting that results in trashy results. So what Haiku does is optimises the prompt.

Paul Murphy   31:28
Yeah.

Diganth Sanghvi   31:33
with the intent.

Paul Murphy   31:36
Cool, good.

Diganth Sanghvi   31:36
and that goes to partition router. So what is partition router?

Ian Ng   31:37
And.

Paul Murphy   31:39
Mm.
Mm-hmm.

Diganth Sanghvi   31:43
It uses now DynamoDB, the partitions that we have created, the metadata that we have stored. It uses that to figure out which files to look at.
In this, I...

Meiyappan Chidambaram   31:56
So this partition router goes and cheques the DynamoDB table.

Diganth Sanghvi   32:00
Yeah, if with the prompt rewrite, it understands which files it needs to grow. Like, suppose like I ask like, right, which one is like paracetamol, blah, blah, blah thing. So it needs to go and look for like this one, the results of the tests and anything. So it will go look for the metadata for these kind of files.

Meiyappan Chidambaram   32:09
But.

Paul Murphy   32:13
Mhm.

Diganth Sanghvi   32:18
And it will figure out, okay, I need to look for, okay, the test results of this. This is the partition present in OpenSearch or cold vector. First of all, that's the section that needs to understand whether it needs to go to OpenSearch or it needs to go to the S3 vectors.

Paul Murphy   32:33
Mm.

Diganth Sanghvi   32:34
Then, in you don't want to scan the entire open source serverless, right? You need to scan just the files that are relevant to this, yeah.

Paul Murphy   32:45
Yep, which is where the metadata comes in to help philtre it out.

Diganth Sanghvi   32:47
That's to philtre it out, so it doesn't stand the full corpus of it.

Meiyappan Chidambaram   32:51
But.

Paul Murphy   32:52
Mm.

Meiyappan Chidambaram   32:53
Sorry, sorry.

Diganth Sanghvi   32:53
So, the metadata filtering happens, and then you scan those things, scan the all the dots, and you retrieve.
In two ways, one with BM 25, the second one is K nearest neighbor. These are two algorithms that can help you find the relevant chunks and both run on parallel.
So that's parallel retrieval for you.

Paul Murphy   33:21
I think May had a question.

Meiyappan Chidambaram   33:23
Oh no, I wanted to ask again, can you repeat again from the partition router which cheques the DynamoDB? Can you explain that part again one more time, please? Thank you.

Diganth Sanghvi   33:23
Yeah.
Yeah.
Yeah, of course. So what DynamoDB that we hold is all the metadata of all the files, right? Okay. So, and from the prompt, we understand, okay, it needs to go and find the case studies of the drug test that we did, right? So it will find all the case, because we have created metadata according to that. Okay, we need to find the case studies like that.

Meiyappan Chidambaram   33:38
Yep.
Yeah.
Yep.

Diganth Sanghvi   33:54
So, okay, it will find these files and these methods. Okay, I need to find the case study partition that we created in open source serverless. Okay, fine. Now I got that information. The partition router gave the large language model to go and find this. And now the algorithm that we are using is
BM25 and KNN. That's semantic search. BM25 is keyword search and both run in parallel. So we get the best of both the worlds.

Meiyappan Chidambaram   34:17
Yep.
Okay.

Diganth Sanghvi   34:27
Right, so let's say...

Meiyappan Chidambaram   34:28
Got it.

Paul Murphy   34:28
Ohh.

Diganth Sanghvi   34:30
So let's say like paracetamol, something like paracetamol that can be easier and faster, that will get the relevant chunk and something like TNN is a semantic search. It figures out all the chunks and then it brings it to agent core. Then we apply re-ranking.

Paul Murphy   34:34
Mhm.

Diganth Sanghvi   34:50
That's another model. That's go ahead, re-rank 3.5. It looks at the part, all the retrieve chunks, and it looks like, okay, no, this is not the top chunk. This is the top chunk.
It's trained to do that. So it's really good at it.

Meiyappan Chidambaram   35:03
Mhm.
Okay.

Diganth Sanghvi   35:08
If it rerimes it.

Meiyappan Chidambaram   35:11
Mm.

Diganth Sanghvi   35:11
H.

Ian Ng   35:14
Stupid question, is this all like features of Agent Core? If I go through Agent Bedrock, Agent Core will give me no? Or this is how you configure it?

Diganth Sanghvi   35:14
So, I, yes.
No.
No.

Meiyappan Chidambaram   35:23
It's a model thing.

Diganth Sanghvi   35:24
It's a model, you need to configure it. It's part of the agent profile framework that you create on your own. That's the custom fusion rack.

Paul Murphy   35:26
Ohh.

Ian Ng   35:27
If.

Paul Murphy   35:34
Ahh.

Meiyappan Chidambaram   35:35
That's, I think you configure everything in the tools and model and everywhere. It's on the agent code is just like a house inside that. Yeah, it's a runtime.

Diganth Sanghvi   35:39
Yes.
It's a runtime.
Yeah, it's a runtime and the agent only gives you observability, identity, memory, guardrails. Yeah, that's it.

Meiyappan Chidambaram   35:53
Like, you can enable the short-term, long-term, and all these things, but the core concept is inside the tools and models.

Diganth Sanghvi   36:00
Yeah, that you need to write.

Ian Ng   36:00
That's good. See me, see me knows a little bit already, so see, that's good. You see, that's exactly what we want, right?

Paul Murphy   36:01
Wright.

Meiyappan Chidambaram   36:02
The.

Diganth Sanghvi   36:05
Yeah, yeah.
Yes.

Paul Murphy   36:07
Cool, cool, cool. Interesting.

Diganth Sanghvi   36:09
And from there, we got the top chunk that the user is asking for. Then Sonnet comes out and be like, okay, I got the chunk. Now I need to make it readable, like how you get the ChatGPT response, right? Yes, I can help you with that, blah, blah, blah, blah, blah, blah, blah. These are the contents.

Paul Murphy   36:11
Mhm.
Mhm.
Mhm.

Diganth Sanghvi   36:28
It does that and it displays to the user.

Meiyappan Chidambaram   36:31
Where the sonnet comes in, which plays sonnet comes in? OK, got it.

Diganth Sanghvi   36:33
In synthesise site.
Yeah, where sonnet is in vote, it gives the chunk is given to sonnet. It writes the response using the chunk.
And it shows to the user if user is happy, we won. If it's sad, then we need to figure it out why what happened.
This is when you question an unstructured data. Now, we come to the retrieval of structured data. So we already spoke about how we are going to do with Snowflake.
So let's say somebody writes, like marketing team writes, okay, how did my AdSense revenue was in last quarter compared to this quarter? So that's a comparison, right? Again, it will text to SQL will happen, like that's a model that will that will use, so we can use Haiku or Sonnet depending on the performance we need to.

Meiyappan Chidambaram   37:31
Yeah.

Diganth Sanghvi   37:31
experiment if haiku is good enough or not.
If it's not, we can swap it for Sonnet. If it is, then let's use Haiku. That's proved to experimentation right there. And agent core gives us a code interpreter. It's A Python code interpreter that we can use to figure out if there is any syntax error because large language models.
Can we sent that setups?

Ian Ng   37:58
Neal.

Diganth Sanghvi   37:58
And yeah.

Ian Ng   38:00
Okay.

Diganth Sanghvi   38:01
Yeah, yeah, I don't excuse that. So we run it, we create the query, and my idea is using Lambda and AWS private link, we pass that query and run it on Snowflake. We fetch the result. Again, SONNet comes in with those results, okay, like this is the data.
This is the revenue. It arranges, it massages the language, does whatever, pushes those data back to the user.
This is, yeah, this is my idea. Yes, Paul, sorry.

Paul Murphy   38:31
And does it?
Cool, does it? So, this the top one here, the agent loop agent called runtime, that's specifically for the unstructured data. Sweet, and the bottom one is specifically for structured data. You don't.

Diganth Sanghvi   38:41
Mhm.
Yes.
Yeah.

Paul Murphy   38:53
Chuck this unstructured data through this particular loop at all do.

Diganth Sanghvi   38:57
No, because when we are, that's the segregation we need to find, like when it asks a question like that can be structured data question, that's where it will find and figure it out. OK, it needs to go to the snowflake part. That's the second agent, sub-agent.

Paul Murphy   38:58
No, OK, great.
Right.
Ah.

Diganth Sanghvi   39:15
It creates, that's a sub-agent from the top.

Paul Murphy   39:16
So there's an...
Right, oh yes, here. So it'll, so I ask a question, the intent and rewrite will go, okay, that part of it's going to be unstructured, that part of it's going to be structured and you 2 paths, off you go. Figure it out.

Diganth Sanghvi   39:30
Yes.
Yeah, yeah, that spins another sub-agent right there. Like its sub-agent won't be active, it spins it up, it gives the task, and then it will go.

Paul Murphy   39:41
Sweet, and I'm assuming.
Those 2 dudes run off, do their thing, and then they come back, give it to another person, another agent, to smush it all together and go, oh, the same agent. Ah, okay, right.

Diganth Sanghvi   39:49
Yeah.
Yeah.
It's the same agent, but another model. Yeah. So agent is basically brain with tools. Now these are the multiple tools that we can utilize, right? So yeah.

Paul Murphy   40:00
Yep.
A lot.
TS.
Mhm.
Cool.

Diganth Sanghvi   40:20
come together. So if you ask a complicated question that needs to hit unstructured and the structured at the same time, it can still do it.

Paul Murphy   40:21
Smoosh it together.
Yep.
Ah, okay.

Diganth Sanghvi   40:29
Yeah, and the best part of this is fishing.

Paul Murphy   40:34
Mm.

Diganth Sanghvi   40:34
The more we use, the cheaper we get.

Paul Murphy   40:39
Oh, that's interesting.

Diganth Sanghvi   40:41
So I'm going to use Amazon MemoryDB and I'm conflicted upon Elasticsearch elastication as well. Dom, you can help me out with that. I have heard both are really good, but if you have any preference, Dom, about this,
I can just change that to elastic caching.

Meiyappan Chidambaram   41:02
You can cheque the call from that as well.

Diganth Sanghvi   41:02
Some more.
Yeah.

Meiyappan Chidambaram   41:05
That will be the first preference, yeah.

Diganth Sanghvi   41:05
Yeah.
Yeah, anything. So right now, MemoryDB. So it's a little bit expensive, but over the period of time, it's cheaper than the token cost. So what happens is, if somebody is querying the same results again and again, you don't need to go and the data hasn't changed. It's already there. Why not use it?

Paul Murphy   41:13
Mhm.
It's already there.

Domenico Campagnolo   41:23
Thank you.

Paul Murphy   41:24
Yeah.
And so that's a different level of, so you got hot tea, cold tea in the unstructured data. That's again, like a, I don't know, boiling tea.

Diganth Sanghvi   41:33
Yeah.
Yeah, so this is the fastest one, caching.

Paul Murphy   41:39
Interesting.

Diganth Sanghvi   41:41
Then, if you are talking about latency, then the anything that we retrieve from the cache will be the fastest.

Paul Murphy   41:43
Mhm.
Oh, yeah.

Diganth Sanghvi   41:49
Yeah.

Paul Murphy   41:50
Awesome.

Diganth Sanghvi   41:50
So that's my idea. Yeah.

Meiyappan Chidambaram   41:52
I think all the answers, whatever the structure and unstructured goes, and then it saves to the memory TB as well, so that when the user next question start and then it cheques the cache, yeah, it will.

Diganth Sanghvi   41:56
Yeah.
Yes.
If it matches, it just picks it up from there itself.

Paul Murphy   42:05
Cool.

Diganth Sanghvi   42:06
And we have a caching policy, like till what period we are going to save the cache. If Novartis is like, cache shouldn't be saved more than 24 hours, let's enable it.

Paul Murphy   42:12
Hmm.

Domenico Campagnolo   42:18
Yes, question. Yeah, of course.

Diganth Sanghvi   42:21
Yeah.

Domenico Campagnolo   42:25
Okay. Did you finish with the presentation? Because I have some question. So about the ingestion and indexing pipeline. So I believe there will be an initial process where we are indexing all the existing data. But then we need to consider also the ongoing.

Diganth Sanghvi   42:28
Yes, of course.

Paul Murphy   42:30
Yeah, go for it.

Domenico Campagnolo   42:43
Ingestion indexing pipeline, right?

Diganth Sanghvi   42:46
Yeah, so ongoing means like the updated documents. So I think that will be out of.

Domenico Campagnolo   42:51
Yeah, new document then arrives, so we want to...

Diganth Sanghvi   42:53
that will be out of the scope for the POV. We can think about it in production, but that pipeline conversion, that's out of the scope for POV. We can't do that in this time.

Domenico Campagnolo   42:56
Okay.
OK, good to know. So, for now, we are considering to work just with existing data and excluding. OK, cool.

Diganth Sanghvi   43:06
Yeah.
Yep.
Exactly.

Domenico Campagnolo   43:17
Makes sense.

Diganth Sanghvi   43:17
Yeah.
Now we can answer all the Paul's questions with that thing. Like, I'm glad I was able to explain it here.

Domenico Campagnolo   43:19
No.

Paul Murphy   43:21
Yeah.
Yeah, this ingestion and indexing is for unstructured data, isn't it? Yeah.

Diganth Sanghvi   43:27
Yeah.
Yeah.
Yes, yes. The only thing from structured we will be ingesting is just the schema of the semantic layer. Like so that it understands like, okay, the large language model understands, okay, if they are asking about the AdSense data, which table I need to go so that it can write the SQL query.

Paul Murphy   43:47
Mm.

Domenico Campagnolo   43:49
Yeah.

Ian Ng   43:53
Qing.

Domenico Campagnolo   43:54
Right, so this is not something that is done on flight.

Diganth Sanghvi   44:00
Yeah.

Domenico Campagnolo   44:00
So we, okay, and also we assume that the scheme will never change at this stage.

Diganth Sanghvi   44:05
Yeah, absolutely. We assume that for pure schema can't change.

Domenico Campagnolo   44:09
Because, you know, the schema always evolve, so you are the new column, new tables, so you drop tables, so probably you're looking some for data that doesn't exist anymore or was moved to someone else's.

Diganth Sanghvi   44:11
Yeah, of course, of course, yeah, yeah.
Yeah. Yeah.
Yeah.
Yeah, for that then we need a continuous like the API call from our MCP connectivity with Snowflake to update as soon as any schema change happens. It needs to update the vector database of that. That's some other level of complication that we can't do in this time.

Domenico Campagnolo   44:41
Yes, so we are focusing just on the one of one of ingestion or whatever, and nothing changes. So, the ongoing eventually will be part of the another thing for this no flex connector between our agent core.

Diganth Sanghvi   44:44
Exactly.
Yeah.
Nothing changes.

Domenico Campagnolo   45:00
And snowflakes, so you, I think, to use a lambda for better performances.

Meiyappan Chidambaram   45:05
And.

Diganth Sanghvi   45:07
Yeah.

Domenico Campagnolo   45:11
which means, so because I'm seeing it, so there could be some sort of a Snowflix connector, so you can use a Python files into your agent core without using Lambda in theory.

Diganth Sanghvi   45:22
Yeah.
Yeah, yes.

Ian Ng   45:24
You still need, you still need connectivity, though.

Domenico Campagnolo   45:25
So, is the lambda a way to eventually scale up and down the end? OK.

Diganth Sanghvi   45:30
Yes.
Yeah.

Domenico Campagnolo   45:33
Right, so we need to figure out how snowflakes eventually offer something better rather than remember the wheel, or this could be an acceptable solution too. So

Diganth Sanghvi   45:39
Yeah.
Exactly my point. So we have to ask how we connect to Snowflake. If they give us access to MCP of Snowflake with that schema and everything, that's the best case scenario for us.

Domenico Campagnolo   45:54
Yes.

Ian Ng   45:56
Who's the who's the snowflake expert?

Diganth Sanghvi   45:56
I'm.

Domenico Campagnolo   45:57
Eventually, we go with this JC and we need to implement our way to run multiple query in parallel and whatever in an efficient way. OK, thanks.

Ian Ng   46:09
Who's the who's the snowflake expert between you and my homemate?

Domenico Campagnolo   46:14
Yeah, of course. Mehul knows Snowflix, but I mean...

Ian Ng   46:16
No, is it is it you or is it Mehul? Because if it's Mehul, then feed it all to him, get him to help with the answer.

Domenico Campagnolo   46:22
Stop using the busy world. We are always busy, but we found the time to do anything.

Diganth Sanghvi   46:25
Yeah.

Paul Murphy   46:26
Yeah.

Diganth Sanghvi   46:28
And while we are at it, if we can ask one more thing to them is like, are we building the semantic layer or they building the semantic layer?

Domenico Campagnolo   46:31
Yeah.

Paul Murphy   46:33
Mhm.

Domenico Campagnolo   46:33
Yeah.
Mm.

Diganth Sanghvi   46:41
So, on basis of that, we'll be using Mehul or Dom.

Ian Ng   46:46
I think the input from Mehul and Dom is important only because Mehul is going to talk to the head of data in the next couple of weeks. So if you pretend that, that helps Sivo as a whole, you know, yeah, that's why I've been bugging you, backfill.

Diganth Sanghvi   46:46
For 2 weeks.
Yeah.
Ng.

Ian Ng   47:04
Mehul in the back end, Dom, so.

Domenico Campagnolo   47:06
Yeah, yeah.

Ian Ng   47:07
Please, I don't ask you to do **** for no reason, mate. So, no, I do, but you know, yeah, yeah, Nicole.

Domenico Campagnolo   47:10
Yeah.
No worries.

Diganth Sanghvi   47:15
Yeah, all good. Yes, let's focus on the questions now, Paul. What are the questions that are still unanswered?

Paul Murphy   47:19
Cool.
Awesome.
Yeah, that's a lot of them answered. Let me just move them out of the way. So how do we get the data? What do we need? Where is kept? I think that's all answered. These might not be good questions that are left. Tell me if they're not.

Diganth Sanghvi   47:25
Yeah.
Yep.

Paul Murphy   47:37
Um...

Ian Ng   47:38
You're gonna be the judge of that, mate. Go for it.

Paul Murphy   47:41
Yeah, so, yeah.

Diganth Sanghvi   47:42
What does that mean? How do I know it's the right data?

Domenico Campagnolo   47:44
Yeah.
Not a quality, yeah, of course.

Paul Murphy   47:48
Data quality. So if I've got a file named Panadol Campaign 3 and someone's saved over the top of it with, I don't know, this is my trip to Japan, how do I know?

Diganth Sanghvi   47:50
Mm.

Ian Ng   47:56
One, two, three.

Diganth Sanghvi   47:57
Of course.
Okay.

Paul Murphy   48:05
In the how does the system know if that's...

Diganth Sanghvi   48:06
Ahh.
Yeah.

Paul Murphy   48:09
Bad or not, because in their manual processes, they'll open it up and go.

Domenico Campagnolo   48:10
Yeah, Dilva.

Diganth Sanghvi   48:10
Yeah.

Domenico Campagnolo   48:13
Yeah, never assume that your source is giving a clean data.

Diganth Sanghvi   48:13
Yeah.
Of course, uh, that's so...

Domenico Campagnolo   48:19
They probably can give you broken data, incomplete, or in this case, overriding a file with the same name. And then probably you have a metadata from the old name, old file, but then the new files contain something different. So yeah, there could be a lot of problems.

Diganth Sanghvi   48:23
Yeah.
True.

Domenico Campagnolo   48:37
The.

Diganth Sanghvi   48:37
Cool.

Ian Ng   48:38
From a purely selfish AI perspective, do we care?

Diganth Sanghvi   48:39
So.
Yes, because that will help us to create the metadata and that will help us to philtre down the partition that we are going to search.

Ian Ng   48:52
Do we?
Let me rephrase it, responsibility of the data sources, data quality, is that us or them?

Domenico Campagnolo   48:58
Yeah. This could be another assumption. So we assume that they provide us the data, a clean, curated and everything else. So if the data is broken, we provide wrong results. So this could be part of the contract.

Ian Ng   49:01
Yeah.
That is quite, yeah.

Paul Murphy   49:08
Mm.
Ken.

Diganth Sanghvi   49:12
Yeah.

Paul Murphy   49:14
And...

Diganth Sanghvi   49:15
Yeah.

Ian Ng   49:15
Because what could happen is that we build everything and then the queries are the answers that we're getting is all messed up, right? Then we do the troubleshooting, we go back, oh.

Diganth Sanghvi   49:21
Yeah.

Paul Murphy   49:21
Hmm.

Ian Ng   49:24
This file is called Panadol 123, but it's called Pictures of Tokyo and Osaka, right? So yeah.

Paul Murphy   49:28
Yeah.
While I agree that it's pro...

Meiyappan Chidambaram   49:31
The intent and rewrite will fix it, isn't it? The intent and rewrite will be able to find it, isn't it?
That kind of.

Diganth Sanghvi   49:38
It can't correct the file name. It can just point it to the right direction. All it can do is like if there are pictures of Osaka and Tokyo, it's just going to ignore it.

Meiyappan Chidambaram   49:40
Yeah.

Ian Ng   49:44
Mm.

Meiyappan Chidambaram   49:44
If.
Yeah.

Ian Ng   49:51
Mm.

Paul Murphy   49:52
Okay, right.

Ian Ng   49:53
Well, if you set it up so it only ingests text or something like that, or...

Paul Murphy   49:54
Interesting.

Diganth Sanghvi   50:00
No, like it's a multimodal thing. So we have to ingest images as well because they are expecting images in their output as well because it's marketing.

Ian Ng   50:01
Alright.
Mm.
Mm.
But how does it know that, you know, for a file name, Pandor123, if it's got a picture of Tokyo Tower, it's like ignore.
OK.
Mm.

Diganth Sanghvi   50:24
Because of the cosine similarity algorithm, it will be part here, so it won't fetch it, yeah, in the vector database.

Ian Ng   50:28
Yep, OK.

Paul Murphy   50:29
Mmm, mmm, mmm.

Ian Ng   50:31
So that means data quality is important, but not that important because you will ignore stuff as well, so yeah.

Paul Murphy   50:32
Um...

Diganth Sanghvi   50:34
Very important.

Paul Murphy   50:36
Yeah.

Diganth Sanghvi   50:37
Yeah.

Paul Murphy   50:39
I've only got, I've got a 4:00. I've only got 5 minutes left.

Diganth Sanghvi   50:44
Yeah.

Ian Ng   50:44
Why have you got a 4:00, mate? Why have you got a 4:00, Paul?

Paul Murphy   50:47
Oh, the API thing, yeah.

Ian Ng   50:49
Sorry, I can't override that for you, mate. So there you go. Yeah.

Paul Murphy   50:52
Yeah, no, that's alright.

Diganth Sanghvi   50:53
Hey, Tan.

Ken Lawrie   50:54
Sorry, guys, I thought this meeting was from 4:00 to 5:00 for some reason.

Ian Ng   50:57
Don't you cheque your calendar, Ken? Come on, man.

Paul Murphy   50:58
Yeah, okay.

Meiyappan Chidambaram   51:00
Yeah.

Paul Murphy   51:00
All good. We've recorded it. Oh yeah, excellent.

Ian Ng   51:00
Anyway, I'm slow, yeah.
I'm happy to, I'm happy to take a five-minute break, and then we can continue. We can, but get Paul stuff done first, so yeah.

Diganth Sanghvi   51:12
Yeah.

Paul Murphy   51:12
Yeah, that.
to say about getting the data quality, thank you, yes, is I think there needs to be a way to surface that before the output comes out. What I'm worried about is trust in the system. At the moment, they've got a level of trust that the manual process works. They've accepted that.

Ian Ng   51:19
Data quality, yeah.
Mm.

Diganth Sanghvi   51:29
Mm.
Mmh.
Of course, yeah.

Paul Murphy   51:38
manual processes have problems, but there's a level of trust there that they know when you can physically handle it and they do it. What I don't want to happen is with feeding data and because we're all, by we I mean Novartis as well, we're all very conscious about the data. We don't feed it anything that's incorrect.

Diganth Sanghvi   51:41
Yep.
Yeah.
Hmm.

Paul Murphy   51:59
and then we leave and then all of a sudden it just collapses in a heap because people got lazy and then they no longer trust the system.

Diganth Sanghvi   52:07
That will be part of the evaluation part, because this needs to be continuously evaluated as well, because the problem with AI is, again, I know I say this a lot, AI fails silently. It will display you things, but it might be absolute dot ****.

Ian Ng   52:08
Is it up?

Paul Murphy   52:11
Okay, Grant.
Sweet.
Yes.
Yeah.
Yeah, and I don't want it to fail. I want it to fail. If we think of this as like a very high level input output.

Diganth Sanghvi   52:30
Yeah, so it needs to be continuously evaluation needs to run on it. So that's what me and Ken were looking at, talking about it in the afternoon as well. So we will let user test it, like for like 10 users across 10 days. So we got a healthy golden data set, right? But what is, what good looks like?

Paul Murphy   52:34
Yeah.
Pass. Cool.
Yeah, cool.
Yeah.

Diganth Sanghvi   52:50
Then we use LLM as a judge to evaluate all the outputs once in one week. Let's say it goes through the corpus that we have.

Paul Murphy   52:51
Yeah.
Yeah.
Yep.
Mm.

Diganth Sanghvi   53:02
It evaluates it. It says that, okay, this was ****, this was ****, this was ****. It generates the report and it produces it. And when everything goes to ****, we know that we need to start looking at like something has gone wrong. Is the ingestion data wrong? Somebody changed the documents. What the **** happened?

Paul Murphy   53:08
Yeah.
TS.
Yeah.

Ian Ng   53:17
We are troubleshooting, yeah.

Paul Murphy   53:19
Yeah, oh, brilliant. No, that's good. And then that that kind of, yeah.

Ken Lawrie   53:20
Yeah.

Diganth Sanghvi   53:21
Yeah.

Ken Lawrie   53:23
And it's going to be hard for us to judge as well, because I'm not an expert in healthcare. I'm not sure if anyone else on the call is an expert in healthcare. So if we see a bad answer, it's going to be hard for us to really identify it.

Ian Ng   53:26
Mm.

Diganth Sanghvi   53:31
Of course.
Yeah.

Ian Ng   53:39
It goes back to the scope, right? What we want to deliver as well, so yeah.

Diganth Sanghvi   53:39
So.
Yeah, yeah, that's why like we let them use and we use the feedback from them, like what we did with ECH, thumbs up, thumbs down, like what good looks like. And until and unless we get to the perfect category that they are happy with, we record all the conversations that they have had previously and give the LLM the idea, okay, this is what good looks like, this is what bad looks like.

Paul Murphy   53:52
Brilliant, good.

Ian Ng   53:55
This.

Diganth Sanghvi   54:04
and let it judge all the responses.

Paul Murphy   54:07
Okay, brilliant. I think that goes some way to answering my questions about the output. Yeah, can I trust it? Thumbs up, thumbs down. That's how you prove it. And then we can learn from that.

Diganth Sanghvi   54:12
Yeah.
Yeah.
Yeah.

Paul Murphy   54:21
And Taylor, okay.

Diganth Sanghvi   54:21
Yeah, that's my bad because I'm working on the evaluation architecture right now and I will push it into confluence in within two days and it can answer most of your questions. Yeah.

Paul Murphy   54:26
Oh, that's cool, yeah.
No, no, that's alright, all good.
Yeah, yeah, no, all good. And some of these questions like, what is my benchmark campaign? Like you said, golden set of data. We won't be able to answer that at the moment. That'll be for them. Yeah.

Ian Ng   54:34
Okay.

Diganth Sanghvi   54:42
Yeah, the.
Yeah, yeah, that the client needs to answer.

Paul Murphy   54:49
Okay, I'm going to have to drop off. I think this was, I don't know about everyone else, but this was very useful for me. I think we've got.

Ian Ng   54:56
Yes, no, it's good.
As long as it, yeah.

Diganth Sanghvi   54:59
Yeah.

Paul Murphy   55:01
Bedrock understanding.

Ian Ng   55:03
Pun, nice one. As long as you've got the data to start figuring out all the workshops that we need to do week one, because in terms of logistics, right, the guy's not going to be with us for week one. It's just going to be yourself and Ken, and Ken's already started on all that.

Paul Murphy   55:05
Very much intended.

Diganth Sanghvi   55:05
Yeah.
Yeah.

Paul Murphy   55:12
Yeah.

Diganth Sanghvi   55:16
Yeah.

Paul Murphy   55:18
Yeah.
Got.

Diganth Sanghvi   55:21
Yeah.

Ian Ng   55:23
In for stuff that he needs. So there you go. Paul, if you have to go, go, I'll stop the recording. Can I ask for a 10 minute break? Is that okay?

Diganth Sanghvi   55:25
Yeah.

Paul Murphy   55:26
Any.
Ohh.

Diganth Sanghvi   55:33
Yes, find it fresh air as well. Yes.

Ian Ng   55:34
Yes, yes, yes, Ken, yeah, yeah, let's go back into this.

Paul Murphy   55:36
Yeah.

Ken Lawrie   55:37
Yep, yep, all good.

Paul Murphy   55:39
Diganth, any chance you can create yourself as an agent that we can just throw at the...
About his users.

Diganth Sanghvi   55:47
That's me, that's me for you.

Paul Murphy   55:48
Nice, OK, cool.

Ian Ng   55:49
That's human in the loop. Yeah, there you go. Okay. Let's dial back into this at 410 for the Ken stuff. Yeah. Thank you.

Paul Murphy   55:51
The.
Awesome.

Diganth Sanghvi   55:56
Yeah.
Amazing. Cool. See ya. Bye bye.

Ken Lawrie   55:58
Sure, OK.

Paul Murphy   55:59
Hey, guys, bye.

Domenico Campagnolo   56:01
See it.

Paul Murphy stopped transcription


## Raw Transcript - Part 2

Novartis Autopilot - Discovery Modelling-20260831_161537-Meeting Recording
31 August 2026, 06:15am
11m 45s

Ian Ng started transcription

Domenico Campagnolo   0:04
Now, it's recording.

Diganth Sanghvi   0:05
There we go, yeah, we start.
So I was doing high level research again, not an export in Snowflake, just Googling stuff across Cortex agent and I found we can trigger Cortex agent if we have Snowflake MCP to Bedrock itself.

Ian Ng   0:19
Yeah.

Ken Lawrie   0:27
Yeah.

Domenico Campagnolo   0:28
Yeah.

Diganth Sanghvi   0:28
So, if...

Ian Ng   0:29
So, you don't need, you don't need the lambda, you don't need the...

Diganth Sanghvi   0:32
Anything. Yeah.

Ian Ng   0:33
Foulon.

Domenico Campagnolo   0:33
But this is a, so I ask you a question about context. So in this case, you lose the control of what is doing.

Diganth Sanghvi   0:36
Yes, yes.
Yes, yes, that's one point. Then we have to maintain it at snowflake level as well. One agent needs to be maintained at snowflake level. One agent, multiple agent needs to be managed in AWS. So that creates a lift between the architecture. But that gives us a major control over because

Domenico Campagnolo   0:46
Okay.

Diganth Sanghvi   1:01
Cortex agent is a fine-tuned agent, like the harness. Inside, it might be using Sonnet or whatever, but the harness is trained. They have made it in Snowflake, so they have a better understanding of everything, how the query runs, what's the most optimised way. Yeah.

Domenico Campagnolo   1:14
Of course, if we use a cortex, we don't need it to maintain the schema on our side and whatever. So we delegate the understand data to, which makes sense to me. So I believe this could be the best approach. If we don't want to have this, so if we want to control all the agents.

Diganth Sanghvi   1:21
Yes, like we delegate everything.
Yes.
Best approach.

Ken Lawrie   1:29
Hello!

Domenico Campagnolo   1:34
We need to treat snowflakes as a just that access point, whatever.

Diganth Sanghvi   1:41
There are just two biggest hangups here is one of the Chitali's comment was we have a registry in AWS where we maintain the agents where we that's the governance standard because we can't get around governance even though it makes sense for us.

Ian Ng   1:42
Mhm.

Domenico Campagnolo   1:56
Right.
Absolutely.

Diganth Sanghvi   2:01
Governance standards are set in stones, and we have to go via that, so...

Ken Lawrie   2:01
Thank you.
And it probably goes against what we've told them we're going to do in the RFQ response as well.

Diganth Sanghvi   2:12
But RFT response was not done by us. That's the biggest, yeah.

Ken Lawrie   2:12
Because that.
Well, I mean, the statement of work, it's kind of pretty specific about what we're going to do. And also the question, do we have any funding from AWS in relation to this project?

Diganth Sanghvi   2:20
Yeah.

Ian Ng   2:27
I.
I don't know, to be honest. I think that's a Sean and Brian question, so...

Ken Lawrie   2:34
Yeah.
Because imagine how ****** *** that would be if we started using slow reflex.

Diganth Sanghvi   2:39
But we are not using anything. We are just connecting Snowflake, technically.

Ken Lawrie   2:41
Yeah.

Domenico Campagnolo   2:43
Yes.

Ken Lawrie   2:44
Well, I mean, I'm sure you'd get that be like billing associated with using it on Snowflake's platform.

Diganth Sanghvi   2:49
Yeah, yeah, yep.

Ian Ng   2:52
I think Snowflake, yeah.

Diganth Sanghvi   2:52
or worst case scenario, if we just get access, if we get access to MCP.

Ken Lawrie   2:54
In.

Diganth Sanghvi   2:58
Let's just connect it to Bedrock and let Bedrock write the query and query it on the schema.

Domenico Campagnolo   3:04
Yeah, in this case, I will avoid lambda because there are some technical issues. It could be better to spin up another solution. I was about to understand what is the best thing, so we could use an easiest task.

Ian Ng   3:04
Can I?

Diganth Sanghvi   3:10
Of course, yeah.

Domenico Campagnolo   3:24
that we can scale up and down because one thing you want to do is you don't know how many query you are running. It could be one, it could be 1000. So in this case, we need a scalability. So you need to have an application load balancer, some easiest tasks that run the query you generate from the rock. So all this infrastructure is dummy. So you just pass the query and you execute the query.

Diganth Sanghvi   3:29
Of course.
TS.
Why?
Oh, okay.

Domenico Campagnolo   3:46
And this connected by JDBC to Snowflix. So Snowflix become like your database, and then you have a horizontal layer that scale up and down based on the number of queries.

Diganth Sanghvi   3:53
Mhm.
Okay, so you mean like if let's say 10,000 people are at the chatbot at the same time and they are all creating the snowflake, the scaling part with Lambda can become tricky. So that's where your solution comes in.

Domenico Campagnolo   4:15
Yes.

Diganth Sanghvi   4:16
Okay.

Ian Ng   4:16
Can we capture this as an alternate?

Diganth Sanghvi   4:20
Yeah, no, no, so.

Ian Ng   4:20
Architecture solution, yeah, in your page, yeah.

Diganth Sanghvi   4:24
No, so Dom, I would suggest like, can you suggest an alternate this one, this pipelining something and that I can add?

Ian Ng   4:25
No.

Domenico Campagnolo   4:30
Yes, yeah, I will go and then I touch, I give you the image that you can attach if you have a presentation on PPT, whatever. Yeah, makes sense.

Ken Lawrie   4:31
Who?

Diganth Sanghvi   4:38
Amazing. Let's do that. Yeah.

Ken Lawrie   4:40
One thing I don't recall seeing is like any talk about the scale, right? Like how many people are going to be using the system? How many marketing people are going to be using it? I can't remember. I remember in the RFQ it does.

Ian Ng   4:40
Yeah, yeah, don't, don't just...

Diganth Sanghvi   4:51
Yeah.

Ken Lawrie   4:59
say that it will need to be scaled up right after the POV, but it doesn't really give any metrics or anything like that.

Diganth Sanghvi   5:07
Yep.
I think those are part of Paul's question.

Ken Lawrie   5:11
Ohh, you're on mute, mate.
Sorry, mate, you were trying to say something?

Diganth Sanghvi   5:15
To that light.

Ken Lawrie   5:19
No, you, I can't hear you, you can no audio.

Domenico Campagnolo   5:20
My user, you're not audible.

Diganth Sanghvi   5:28
Ian.
I can't hear you, Mehul.

Domenico Campagnolo   5:30
So when we talk about scalability, we shouldn't consider the user, because between the user and the Snowflix, there is a system. So maybe the user is trying to retrieve one report or whatever, but the system generated 10 queries out of that question. So we need to consider the something to
blow up in the middle. So maybe 1000 user, concurrent user, can generate 10,000 queries. It's just an example. Makes sense.

Ian Ng   5:59
Is it part of the scope of the is it part of the scope of the proof of value though? That's the thing.

Diganth Sanghvi   6:00
Yeah.

Domenico Campagnolo   6:04
No.

Diganth Sanghvi   6:04
No, no. So, but we need to understand how many people will be creating this POV at least, so that we can scale for at least that much. The architecture takes care of that load at least. So I think that's part of Paul's question, like, who are gonna be creating this tool, chatbot, whatever you want to call it.
Second question is, like, what's the nature of the query?
and how they query it. So these are the part of things.

Ken Lawrie   6:30
Yeah.
Yeah, because we could like request some data and it might make 100 API calls to get the data you request, depending on how they're indexed, their data, you know. So, yeah.

Diganth Sanghvi   6:39
Yeah.
What API calls are you can I lost you there?

Ken Lawrie   6:52
Yeah, so like if you're querying data from Snowflake, like you could have a query which returns you what you want, but you could also do a query for some data that's not part of the index, right? So it might end up making a, you know, 100 API calls to get the information you want.

Diganth Sanghvi   6:57
Mm.
Mm.
Mhm.
Mm mm.

Ken Lawrie   7:11
So, it might so one person could generate quite a lot of traffic depending on what that night looks like, so we don't really know, so we don't know how we need to scale without, you know, knowing all those finer details of what we're trying to do.

Diganth Sanghvi   7:16
Yeah.
Mhm.

Ian Ng   7:28
If it fit into Paul, he'll he'll do the initial analysis with the customer and stuff like that as well for us, so that's the game plan that I have been told to execute.

Diganth Sanghvi   7:28
We would be.
Yeah.
Mhm.

Ken Lawrie   7:41
How's your audio now, Mayer?
So, playing up.

Meiyappan Chidambaram   7:45
Oh yeah, can you hear me now? No, I just want to cheque what you mentioned was like, what is the success criteria for this POV, right? That's what you want to understand. Okay.

Ken Lawrie   7:46
Yeah, there we go. There you go.

Domenico Campagnolo   7:47
Yep.

Diganth Sanghvi   7:47
Yep.
Yeah.
Yeah.

Ken Lawrie   7:56
Yeah.

Diganth Sanghvi   7:58
Yeah, and uh...
If the data is not there in the schema, I feel like it will generate a null result. So basically we are not going to give access to the whole snowflake to our agent, only a particular subsection of it, which, yeah, yeah. So if something is not found in that particular subsection, that's it.

Domenico Campagnolo   8:14
Makes sense, of course, yeah.

Diganth Sanghvi   8:21
0 result. It's not allowed to run it on anything else because Snowflake can be expensive as well.

Domenico Campagnolo   8:30
Yeah, accessing the data as a cost, of course, yeah.

Diganth Sanghvi   8:30
So, yeah.
Yeah, yeah.
So we need to keep that in mind as well. Yeah.

Domenico Campagnolo   8:37
Yeah, you need to define which domain of data you want to access, you want to give access to your agent core.

Meiyappan Chidambaram   8:38
It.

Diganth Sanghvi   8:40
Yeah.
Yeah.
Exactly.

Meiyappan Chidambaram   8:47
Do we have an architecture cost for this, the whole thing? Did we give it to them or do they have any expectation?

Diganth Sanghvi   8:47
Ohh.
Yeah, they created a dummy sheet like what they are expecting and we created the cost model on that and we sent it back to them. So it's part of the SharePoint, the one that I shared with you, May, I think.

Meiyappan Chidambaram   9:10
Okay, okay.

Diganth Sanghvi   9:12
But it's fairy tale numbers. They think the POV will cost less, but testing will cost more and production will cost even lesser. So, yeah, so it's upside down because production is a controlled way, controlled environment.
I feel testing we need to test as well, so the cost will be more. We have added that caveat to them. We have communicated that to them.

Meiyappan Chidambaram   9:35
See.
Sure.

Ken Lawrie   9:43
And are they kind of kind of looking at the AI model costs or the data costs as well, like all the back-end costs that...

Diganth Sanghvi   9:43
Yeah.
Yeah.

Domenico Campagnolo   9:49
Thank you.

Ken Lawrie   9:56
Yeah.
Mhm.

Domenico Campagnolo   10:05
And.

Ian Ng   10:05
Stuff, yeah.

Diganth Sanghvi   10:06
Yeah.
I will forward that to you and just have a look. But those numbers that you see, the amount of tokens that's written there, it's not from me, it's from them.
So I gave a process estimate based on the assumptions.

Ken Lawrie   10:20
Bye.
Okay.

Ian Ng   10:24
Autopilot, AWS pricing, Civo vendor copy. I think that's the one.

Diganth Sanghvi   10:28
Yeah.

Ian Ng   10:29
That's in SharePoint already, mate, yeah.
Doesn't make any sense, but we'll get there.

Diganth Sanghvi   10:41
Yep.

Ian Ng   10:43
Um...

Diganth Sanghvi   10:50
Yeah.

Ken Lawrie   10:51
Has Grant been joining these regular meetings that we've been having?

Ian Ng   10:58
Initially, I think as part of the pre-sales process, yes, but these ones not so much because it's already across.

Diganth Sanghvi   10:58
If I'm...

Ken Lawrie   10:59
Yep.
Yeah.

Diganth Sanghvi   11:04
I think he's on leave.

Ian Ng   11:07
He was on leave, yeah. He was on leave, yeah.

Ken Lawrie   11:07
Okay.

Diganth Sanghvi   11:07
I thought I was wrong, yeah.
Yeah.

Ian Ng   11:17
Do you need anything else, Ken? If not, can we call it? I've got other stuff I need to do as well, so yeah.

Ken Lawrie   11:19
Not at the moment, I'm all good for now.
Yeah.

Ian Ng   11:25
There you go. I will be in tomorrow and Wednesday. If you guys want to come in on Wednesday to support the gun and eat free pizza, please. Thank you very much.

Diganth Sanghvi   11:34
Oh ****, I need to work on that.

Domenico Campagnolo   11:34
Yeah.

Ian Ng   11:36
Yeah.

Domenico Campagnolo   11:36
Free pizza, Wednesday. OK, no for the God, that was for pizza.

Diganth Sanghvi   11:38
Okay.

Ken Lawrie   11:38
Yeah.

Ian Ng stopped transcription
