# AI Infrastructure Pre-Requisites

## My Notes

how much of the foundational components are in place

- what does your account structure look like
- do you have bedrock
- do you have the major bedrock components


are you expecting us to provision install all of these componetent or will there be an internal teamthat does this or has it already been done as part of previous work

call out the cloudfront waf

** lift the checklist up to an approach rather than a detail list **
		we don't need to list every service thatw e want to use rather identify what is currently available and how we get access to services if we need to 





## Raghu's notes


- [ ] AWS account and Org Pre-req. , IAM Permissions, - Ken Lead - Ask question to Novartis 
  - [ ] IA A requirements
  - [ ] Will we be given enough permissions to deploy
  - [ ] WAF 
  
- [ ] Bedrock related - Jeff 
  - [ ] Do you have Bedrock deployed / Model deployed 
  - [ ] Agentcore 
  - [ ] Do you have KB setup 
  - [ ] If they dont are they expected to be done by us or they ha ve the engineers to do that 
  
- [ ] Data - Paul and Jeff
  - [ ] Sample data format for each data source  
  - [ ] Contacts for the data sources
  - [ ] How are we gonna connect to this data soruces 
  - [ ] Are they gonna put a data into a source/S3 where we can access 
  - [ ] Vectro DB's out of data soruces 

- [ ] UI
  - [ ] Expectaion on ownership 
  - [ ] Complexity  

## Transcript

Paul Murphy
0 minutes 3 seconds0:03
Paul Murphy 0 minutes 3 seconds
Uh, yeah.
JV

Jeff Voigt
0 minutes 3 seconds0:03
Jeff Voigt 0 minutes 3 seconds
It's in the SOW, it's in the it's in the proposal.
Jeff Voigt 0 minutes 7 seconds
Yes.

Ian Ng
0 minutes 7 seconds0:07
Ian Ng 0 minutes 7 seconds
No, okay, I just have to ask.

Paul Murphy
0 minutes 8 seconds0:08
Paul Murphy 0 minutes 8 seconds
Give me two secs.
JV

Jeff Voigt
0 minutes 10 seconds0:10
Jeff Voigt 0 minutes 10 seconds
But I was probably going through in more detail.

Ian Ng
0 minutes 14 seconds0:14
Ian Ng 0 minutes 14 seconds
No, no, wait. Where you were before was the infrastructure stuff. I think we get the infrastructure stuff out of the way, then we can focus down on the data points and stuff like that. Agree, disagree? Three, two, one. Okay, here you go. Yeah.

Paul Murphy
0 minutes 23 seconds0:23
Paul Murphy 0 minutes 23 seconds
Okay.
JV

Jeff Voigt
0 minutes 24 seconds0:24
Jeff Voigt 0 minutes 24 seconds
Yeah.
Jeff Voigt 0 minutes 26 seconds
Yep.

Paul Murphy
0 minutes 27 seconds0:27
Paul Murphy 0 minutes 27 seconds
Cool.
Paul Murphy 0 minutes 29 seconds
Okay, so we're ignoring the account structure for the minute.

Raghu Ramireddy
0 minutes 31 seconds0:31
Raghu Ramireddy 0 minutes 31 seconds
I.

Ian Ng
0 minutes 34 seconds0:34
Ian Ng 0 minutes 34 seconds
I think it's worth asking if they have a planning zone and stuff like that, because that helps a lot with all the other stuff that we're doing. But I would defer to Ken. I'm the dumb delivery lead here, not the infra engineer.
JV
Jeff Voigt
0 minutes 36 seconds0:36
Jeff Voigt 0 minutes 36 seconds
X.
KL
Ken Lawrie
0 minutes 48 seconds0:48
Ken Lawrie 0 minutes 48 seconds
Have we been offered any kind of architecture documents that covers their existing AWS environment?
JV
Jeff Voigt
0 minutes 55 seconds0:55
Jeff Voigt 0 minutes 55 seconds
No.
KL
Ken Lawrie
0 minutes 55 seconds0:55
Ken Lawrie 0 minutes 55 seconds
Or would we need to do a discovery of some sort?
DS
Diganth Sanghvi
0 minutes 59 seconds0:59
Diganth Sanghvi 0 minutes 59 seconds
No, I don't.
JV
Jeff Voigt
0 minutes 59 seconds0:59
Jeff Voigt 0 minutes 59 seconds
Or switching it around the other way, we're saying this is what we need to have set up, and we want them to take it away and come back and say what they've got and what they don't need.

Ian Ng
1 minute 10 seconds1:10
Ian Ng 1 minute 10 seconds
So ask the question. Just ask the question. Paul, if you don't mind capturing that, yeah.

Raghu Ramireddy
10 minutes 55 seconds10:55
Raghu Ramireddy 10 minutes 55 seconds
The same part of the bedrock, yeah.

Paul Murphy
10 minutes 58 seconds10:58
Paul Murphy 10 minutes 58 seconds
Mm.
JV
Jeff Voigt
10 minutes 59 seconds10:59
Jeff Voigt 10 minutes 59 seconds
So, that's the core; those are the core services we need to have provision, Trent. That's kind of the to be the main bit.

Paul Murphy
11 minutes 3 seconds11:03
Paul Murphy 11 minutes 3 seconds
Yeah.
KL
Ken Lawrie
11 minutes 4 seconds11:04
Ken Lawrie 11 minutes 4 seconds
So one thing that's on the list here, US East 1 for WAF. So that's one thing that might be out of the ordinary for them because it uses a different, potentially different region than what they're using.

Paul Murphy
11 minutes 5 seconds11:05
Paul Murphy 11 minutes 5 seconds
Yeah.
Paul Murphy 11 minutes 23 seconds
Mhm.
KL
Ken Lawrie
11 minutes 24 seconds11:24
Ken Lawrie 11 minutes 24 seconds
So the rest of it is just pretty much standard.

Raghu Ramireddy
11 minutes 28 seconds11:28
Raghu Ramireddy 11 minutes 28 seconds
Awesome. Yep.

Paul Murphy
11 minutes 30 seconds11:30
Paul Murphy 11 minutes 30 seconds
Hope so, maybe we can.

Raghu Ramireddy
11 minutes 30 seconds11:30
Raghu Ramireddy 11 minutes 30 seconds
Have a look, Ken, if there's anything else that you wanted to touch base. We got 55 minutes, so let's best make you the best of the time.
JV
Jeff Voigt
11 minutes 33 seconds11:33
Jeff Voigt 11 minutes 33 seconds
Yeah.
KL
Ken Lawrie
11 minutes 36 seconds11:36
Ken Lawrie 11 minutes 36 seconds
Yeah, I just think it's a bit of an unusual approach myself. It's like, you know, going to them day one with a list of IAM requirements required. I mean, it's good information for them. I'm sure they're happy to get that information. But I mean, unless we're...

Ian Ng
11 minutes 45 seconds11:45
Ian Ng 11 minutes 45 seconds
Okay.
JV
Jeff Voigt
11 minutes 49 seconds11:49
Jeff Voigt 11 minutes 49 seconds
Just.
KL
Ken Lawrie
11 minutes 54 seconds11:54
Ken Lawrie 11 minutes 54 seconds
expecting that they're not going to give us S3 access, for example. I mean, is that within the realms of possibility that we're going to say, no, we don't approve S3 for use? I'm just wondering what we're trying to achieve by coming up with like a big long list of all AWS services we could

Raghu Ramireddy
11 minutes 58 seconds11:58
Raghu Ramireddy 11 minutes 58 seconds
Hmm.

Ian Ng
12 minutes12:00
Ian Ng 12 minutes
Yeah.
JV
Jeff Voigt
12 minutes 6 seconds12:06
Jeff Voigt 12 minutes 6 seconds
No, I wouldn't have thought so.
KL
Ken Lawrie
12 minutes 15 seconds12:15
Ken Lawrie 12 minutes 15 seconds
potentially use. It just seems like a bit of a waste of everyone's time.

Raghu Ramireddy
12 minutes 19 seconds12:19
Raghu Ramireddy 12 minutes 19 seconds
Exactly, yeah.
KL
Ken Lawrie
12 minutes 20 seconds12:20
Ken Lawrie 12 minutes 20 seconds
To Ben.

Ian Ng
12 minutes 20 seconds12:20
Ian Ng 12 minutes 20 seconds
This this this project readiness checklist, Diganth, is that specific for ECH?
DS
Diganth Sanghvi
12 minutes 27 seconds12:27
Diganth Sanghvi 12 minutes 27 seconds
Yes, most likely, yes, yeah.

Ian Ng
12 minutes 29 seconds12:29
Ian Ng 12 minutes 29 seconds
OK.
Ian Ng 12 minutes 30 seconds
So we're using it as a guide, as a template for Novartis this afternoon. Mel's probably going to have a whole different set of requirements, well, some changes because he'll be with Big Rose. So there you go. Okay.
DS
Diganth Sanghvi
12 minutes 36 seconds12:36
Diganth Sanghvi 12 minutes 36 seconds
Yeah.
MK
Malinda Kapuruge
12 minutes 37 seconds12:37
Malinda Kapuruge 12 minutes 37 seconds
But.
DS
Diganth Sanghvi
12 minutes 44 seconds12:44
Diganth Sanghvi 12 minutes 44 seconds
Mm.

Raghu Ramireddy
12 minutes 45 seconds12:45
Raghu Ramireddy 12 minutes 45 seconds
Yeah, it's gonna take this out.
JV
Jeff Voigt
12 minutes 45 seconds12:45
Jeff Voigt 12 minutes 45 seconds
Is that is that is that true? I I don't think that is true. I think, again, what we use for ECH is exactly what we're going to use for the big beds and exactly what we're going to use for Nevadas, isn't it?

Ian Ng
12 minutes 47 seconds12:47
Ian Ng 12 minutes 47 seconds
But.
DS
Diganth Sanghvi
12 minutes 58 seconds12:58
Diganth Sanghvi 12 minutes 58 seconds
Not entirely true, Jeff, like we will, we will be using it.



MK
Malinda Kapuruge
13 minutes13:00
Malinda Kapuruge 13 minutes
Almost, I would say.
JV
Jeff Voigt
13 minutes 1 second13:01
Jeff Voigt 13 minutes 1 second
Just.
DS
Diganth Sanghvi
13 minutes 2 seconds13:02
Diganth Sanghvi 13 minutes 2 seconds
Little bit difference in the approach because with what we used in ECH would be slightly different than what is because ECH was a naive rag.
JV
Jeff Voigt
13 minutes 4 seconds13:04
Jeff Voigt 13 minutes 4 seconds
Yes.
Jeff Voigt 13 minutes 5 seconds
TS.
DS
Diganth Sanghvi
13 minutes 13 seconds13:13
Diganth Sanghvi 13 minutes 13 seconds
And this time with Novartis, we are going for more of a hybrid approach. So there are a few components that I have added in the architecture as well.
JV
Jeff Voigt
13 minutes 22 seconds13:22
Jeff Voigt 13 minutes 22 seconds
Okay, so it's what we did at ECH plus a few extra components based on the additional complexity of Westfarmers and Novartis. But this should be the complete list that we're going to go into both Novartis and Westfarmers with.
DS
Diganth Sanghvi
13 minutes 25 seconds13:25
Diganth Sanghvi 13 minutes 25 seconds
Yeah.
Diganth Sanghvi 13 minutes 27 seconds
Yes.
Diganth Sanghvi 13 minutes 29 seconds
Yes.
Diganth Sanghvi 13 minutes 35 seconds
West Farmers, sorry.
JV
Jeff Voigt
13 minutes 37 seconds13:37
Jeff Voigt 13 minutes 37 seconds
Oh.
DS
Diganth Sanghvi
13 minutes 38 seconds13:38
Diganth Sanghvi 13 minutes 38 seconds
Uh, the truth, you mean?
JV
Jeff Voigt
13 minutes 40 seconds13:40
Jeff Voigt 13 minutes 40 seconds
Novartis and Vic Rhodes and probably Wesfarmers as well. Yeah.

Raghu Ramireddy
13 minutes 40 seconds13:40
Raghu Ramireddy 13 minutes 40 seconds
The new engagement.
DS
Diganth Sanghvi
13 minutes 43 seconds13:43
Diganth Sanghvi 13 minutes 43 seconds
Okay, this is.
JV
Jeff Voigt
13 minutes 44 seconds13:44
Jeff Voigt 13 minutes 44 seconds
But what I'm getting back to is there's nothing in this list that they don't, that we know up front now that we're asking for that they don't actually need. This is the minimum. Yes, okay. And there might be some more stuff. That's fine. We'll work through the details later. But this is kind of our minimum viable.
DS
Diganth Sanghvi
13 minutes 52 seconds13:52
Diganth Sanghvi 13 minutes 52 seconds
Minimum, yeah, this is the minimum. There will be might be something more to it, but...
Diganth Sanghvi 13 minutes 57 seconds
Yeah.
Diganth Sanghvi 14 minutes
Yeah.

Raghu Ramireddy
14 minutes 2 seconds14:02
Raghu Ramireddy 14 minutes 2 seconds
Yeah, yeah, let's stay on this. Yeah, can anything around connectivity and networking that you want to touch base on? I'm just making a note of keywords, so that should give you enough, you know, indication.
JV
Jeff Voigt
14 minutes 3 seconds14:03
Jeff Voigt 14 minutes 3 seconds
Components can even install.
DS
Diganth Sanghvi
14 minutes 3 seconds14:03
Diganth Sanghvi 14 minutes 3 seconds
Yep.
Diganth Sanghvi 14 minutes 5 seconds
This is a good baseline.
JV
Jeff Voigt
14 minutes 6 seconds14:06
Jeff Voigt 14 minutes 6 seconds
S.
DS
Diganth Sanghvi
14 minutes 7 seconds14:07
Diganth Sanghvi 14 minutes 7 seconds
Yeah.
JV
Jeff Voigt
14 minutes 8 seconds14:08
Jeff Voigt 14 minutes 8 seconds
Yes.
Jeff Voigt 14 minutes 12 seconds
H.
Jeff Voigt 14 minutes 14 seconds
Well, let's talk about the data sources next. That's the other big thing.

Raghu Ramireddy
14 minutes 18 seconds14:18
Raghu Ramireddy 14 minutes 18 seconds
Yeah, yeah, yeah. We'll just crawl through everything and then start capturing.
JV
Jeff Voigt
14 minutes 22 seconds14:22
Jeff Voigt 14 minutes 22 seconds
TS.
MK
Malinda Kapuruge
14 minutes 23 seconds14:23
Malinda Kapuruge 14 minutes 23 seconds
Before we move on, in the previous list, do you think ECR is an important service that we need to have because most of the Agent 4 stuff we push to a sort of like image and then yeah.

Paul Murphy
14 minutes 23 seconds14:23
Paul Murphy 14 minutes 23 seconds
Ohh.
JV
Jeff Voigt
14 minutes 23 seconds14:23
Jeff Voigt 14 minutes 23 seconds
Okay.
Jeff Voigt 14 minutes 25 seconds
No charge.

Diganth Sanghvi
14 minutes 36 seconds14:36
Diganth Sanghvi 14 minutes 36 seconds
We can do that, like it depends on the how complex you want to do it. Like it's good for productionization practise as well. We are using an image and deployment using ACCR, but that depends on how we want to deploy the infra.
SR

Srikar Srinivas Rao
14 minutes 55 seconds14:55
Srikar Srinivas Rao 14 minutes 55 seconds
But I thought in ECH you were using runtime, right? And runtime prerequisite is ECR, correct?
DS
Diganth Sanghvi
14 minutes 58 seconds14:58
Diganth Sanghvi 14 minutes 58 seconds
Yeah.
Diganth Sanghvi 15 minutes 1 second
Um, that would be Denny. I have no idea about that, but yeah. Yeah.
SR

Srikar Srinivas Rao
15 minutes 4 seconds15:04
Srikar Srinivas Rao 15 minutes 4 seconds
Yeah, OK. Maybe it's good to put that in the list. I think, yeah, that's correct.
JV
Jeff Voigt
15 minutes 5 seconds15:05
Jeff Voigt 15 minutes 5 seconds
Stop.

Ian Ng
15 minutes 5 seconds15:05
Ian Ng 15 minutes 5 seconds
Ken.
JV
Jeff Voigt
15 minutes 6 seconds15:06
Jeff Voigt 15 minutes 6 seconds
S.
MK
Malinda Kapuruge
15 minutes 8 seconds15:08
Malinda Kapuruge 15 minutes 8 seconds
Yeah, I'm not familiar with the Nomadis one, but if you're using agent, common pattern is using ECR.
MK
Malinda Kapuruge
31 minutes 42 seconds31:42
Malinda Kapuruge 31 minutes 42 seconds
Yeah, yeah, yeah. See how you guys go because I can't attend 5:00. I don't think I've been invited, right?

Raghu Ramireddy
31 minutes 43 seconds31:43
Raghu Ramireddy 31 minutes 43 seconds
And.

Paul Murphy
31 minutes 48 seconds31:48
Paul Murphy 31 minutes 48 seconds
Yeah.

Ian Ng
31 minutes 49 seconds31:49
Ian Ng 31 minutes 49 seconds
No, this is 5:00 is for Novartis, mate. I think you're here for the Vicro stuff, right? Just to prepare you.
MK
Malinda Kapuruge
31 minutes 52 seconds31:52
Malinda Kapuruge 31 minutes 52 seconds
Not, okay, alright.

Paul Murphy
31 minutes 53 seconds31:53
Paul Murphy 31 minutes 53 seconds
Yeah.
MK
Malinda Kapuruge
31 minutes 55 seconds31:55
Malinda Kapuruge 31 minutes 55 seconds
Yeah, yeah, just to understand, yeah, cool.
Malinda Kapuruge 32 minutes
Yeah, another thing that we didn't touch on is the interaction model. I'm not sure whether it's like human to AI or machine to AI or both. So those are the things we need to exploit as usually.

Paul Murphy
32 minutes 15 seconds32:15
Paul Murphy 32 minutes 15 seconds
Hmm.
Paul Murphy 32 minutes 16 seconds
But this.
JV
Jeff Voigt
32 minutes 17 seconds32:17
Jeff Voigt 32 minutes 17 seconds
What do you, what do you mean?
MK
Malinda Kapuruge
32 minutes 20 seconds32:20
Malinda Kapuruge 32 minutes 20 seconds
The interaction model, for example, is it like a chat bot, like a human interacting with the AI, or is it like a machine interacting with the AI machine to machine versus human to machine?

Paul Murphy
32 minutes 26 seconds32:26
Paul Murphy 32 minutes 26 seconds
11.
JV
Jeff Voigt
32 minutes 30 seconds32:30
Jeff Voigt 32 minutes 30 seconds
Yeah, so these are both, these are both basically what I would consider agentic platforms with human in the loop.
Jeff Voigt 32 minutes 37 seconds
So there's going to be probably a bunch of processing and jobs and agents that collect all the data, analyse the data, and format it all, and then present it up to a user. Whether that's going to be a chat interface or something more structured is yet to be done. And then they'll click on a yes, send that e-mail, yes, generate that marketing campaign.
MK
Malinda Kapuruge
32 minutes 37 seconds32:37
Malinda Kapuruge 32 minutes 37 seconds
Right.
Malinda Kapuruge 32 minutes 45 seconds
Bye.
JV
Jeff Voigt
32 minutes 58 seconds32:58
Jeff Voigt 32 minutes 58 seconds
So there's always going to be a human in the loop.
MK
Malinda Kapuruge
33 minutes 1 second33:01
Malinda Kapuruge 33 minutes 1 second
Right, OK.
JV
Jeff Voigt
33 minutes 1 second33:01
Jeff Voigt 33 minutes 1 second
but with multiple agents, forcing, analyzing, presenting data.
MK
Malinda Kapuruge
33 minutes 4 seconds33:04
Malinda Kapuruge 33 minutes 4 seconds
Yeah, because, yeah, how we, yeah, how we establish the agent identity change based on that interaction model, getting like, you know, session tokens, et cetera, but yeah, details.

Raghu Ramireddy
33 minutes 22 seconds33:22
Raghu Ramireddy 33 minutes 22 seconds
Okay.
Raghu Ramireddy 33 minutes 27 seconds
If nothing else, we can call it a close.

Paul Murphy
33 minutes 30 seconds33:30
Paul Murphy 33 minutes 30 seconds
Nope.
MK
Malinda Kapuruge
33 minutes 31 seconds33:31
Malinda Kapuruge 33 minutes 31 seconds
Cool.
JV
Jeff Voigt
33 minutes 32 seconds33:32
Jeff Voigt 33 minutes 32 seconds
Thanks, everyone. Appreciate it.

Raghu Ramireddy
33 minutes 32 seconds33:32
Raghu Ramireddy 33 minutes 32 seconds
Thanks, guys. See you at 5.

Paul Murphy
33 minutes 33 seconds33:33
Paul Murphy 33 minutes 33 seconds
Awesome.
MK
Malinda Kapuruge
33 minutes 34 seconds33:34
Malinda Kapuruge 33 minutes 34 seconds
Bye.

Paul Murphy
33 minutes 34 seconds33:34
Paul Murphy 33 minutes 34 seconds
See you guys, bye.
DS
Diganth Sanghvi
33 minutes 35 seconds33:35
Diganth Sanghvi 33 minutes 35 seconds
Yeah, see ya, bye.

Raghu Ramireddy
33 minutes 35 seconds33:35
Raghu Ramireddy 33 minutes 35 seconds
It.
JV
Jeff Voigt
33 minutes 36 seconds33:36
Jeff Voigt 33 minutes 36 seconds
Yeah.