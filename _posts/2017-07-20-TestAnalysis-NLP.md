---
layout: post
category : 测试
title: The NLP Meta Model
tagline: Test Analysis
tags: Test
---

**What can the NLP Meta Model do for testers?** 

	“…the goal of the Meta Model is not to find the ‘right’ answers, but rather to
	ask better questions – to widen our map of the world rather than to find the
	‘right map of the world’. The purpose of the Meta Model inquiry system is to
	help identify missing links, unconscious assumptions and reference experiences
	that make up the ‘deeper structure’ of our conscious models of the world.”
	                                           NLP Encyclopaedia,Robert Dilts 

The process of software development is a process of human communication. Beliefs about software are communicated as requirements. Software specifications are based on the requirements and are in turncommuni cated to programmers. Programmers communicate their beliefs about the software specifications to the computer in the form of program code. 

Each of these communication steps may involve several different people and different communication media, both written and verbal. Each communication step is subject to ambiguity, and ambiguity can result in software defects. 

The Meta Model provides testers with a simple model of:

- 3 ambiguity generating transformations – deletion, distortion and generalization

- 12 easily identifiable communication violations resulting from the transformations 

![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/1-2017-07-20-TestAnalysis-NLP.png) 

The paper will provide an overview of the NLP Meta Model and explore ways in which knowledge of the Meta Model can be useful to testers: 

- Apply the Meta Model to the communication we receive, identify ambiguity and possible mistakes early, before being encoded in the system

- Apply the Meta Model to the communication we give to,

&nbsp;&nbsp;&nbsp;&nbsp;o Improve defect reporting

&nbsp;&nbsp;&nbsp;&nbsp;o Help people understand what we aim to do

&nbsp;&nbsp;&nbsp;&nbsp;o Improve development team/test team relationships

- Apply the Meta Model to the system to identify areas of test and as a test derivation aid

&nbsp;&nbsp;&nbsp;&nbsp;o “The docs say the system will always do this, if I can find a situation under which it doesn’t then...”

&nbsp;&nbsp;&nbsp;&nbsp;o What is presupposed by this dialog? How can I invalidate that presupposition?

- Teach testers an easy, effective and repeatable approach to documentation analysis

- Apply the Meta Model to our beliefs about our environment and provide a framework for thinking about our context of testing

**What is the NLP Meta Model?**

The NLP Meta Model is a system of inquiry to explore areas of communication that are susceptible to ambiguity. 

**An introduction**  

“The Structure of Magic” [SoM1], by Richard Bandler and John Grinder, is a study of the language and the application of language in therapy of the 1970s. [SoM1] provides a formal model of the language used, enabling it to be easily understood, taught and used by therapist and non-therapists alike.
The formal model produced, called the Meta Model, was constructed using the theories of transformational grammar. Under transformational grammar, our communication, what we say and write, are surface structure representations of a richer model of the world (deep structure). And we map
our deep structure onto the surface structure by a process of transformation. 

The formal model produced, called the Meta Model, was constructed using the theories of transformational grammar. Under transformational grammar, our communication, what we say and write, are surface structure representations of a richer model of the world (deep structure). And we map our deep structure onto the surface structure by a process of transformation. 

![pic2](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/2-2017-07-20-TestAnalysis-NLP.png) 

For example, if I tell you that “I bought a computer”. Then the statement “I bought a Computer” is a surface structure representation of a richer deep structure. The deep structure contains all my memories of buying a computer, including:

- The strategies I used to decide which computer to buy,

- Memories of all the computers I discarded before choosing the one that I chose,

- How I felt buying the computer,

- How much it cost,

- If I thought it value for money,

- What the shop/salesman was like,

- Where I bought it from,

- What the shop smelt like,

- How I felt carrying the computer home. 

All of this information has been deleted in my transformation from the Deep Structure to the surface structure statement. But all that information is accessible to you if you ask the right questions.

Over time, my computer may crash, have problems with the hardware, and I may come to feel that the computer wasn’t as good a deal as I thought and I may start saying “I bought a bad computer” and may start feeling bad about it. At this point I am not only deleting information, about it being the best computer I could buy at the time, I am distorting my deep structure; and that distortion is having an effect on my real world interaction. But the deep structure is still accessible, and with the correct questioning, I may come to realise that “I bought the best computer at the time”, and that “the computer I bought, now has some problems” which is a different surface structure representation of the same deep structure and may have a different effect on my outlook. 

The Meta Model categorises the transformations from deep structure to surface structure as deletions, generalizations and distortions.

Recognition of Distortion, Deletion and Generalization in a communication allows us to apply a high level analysis to a statement and ask questions to identify:

- What is missing from this communication that might cause problems,  
- What is distorted in this communication, and presented, without adequate definition  
- What is generalized in this communication and what is implied by that generalization  

The Meta Model categories are further divided into 12 elements: nominalization, cause and effect, mind reading, complex equivalence, lost performative, universal quantifier, modal operator, presupposition, simple deletion, comparative deletion, referential index and unspecified verbs. Each of these elements will be discussed and explained later in the paper.

Each of the 12 elements correspond to recognizable language patterns that are found in human communication and have associated questions that can be asked in order to derive more information about the deep structure associated with the communication. 

![pic3](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Test/3-2017-07-20-TestAnalysis-NLP.png) 

Some reasons why all of this is important are:  

- Human communication can be ambiguous  
- Ambiguity can adversely affect the communication and the communicator  
- It is possible to be taught to recognize ambiguity  
- It is possible to respond to ambiguity to gain clarification  
- Clarification can help both the communicator and the communication  
- Software development is a process of human communication   

Most software testers will have encountered requirements, which seemed to be understood by everyone but, which every role in the development process had a different interpretation of. This probably resulted in software defects, delays, unhappy users, rework and a whole host of follow on problems.

This is not just a problem with requirements; specifications, defect reports, test strategies, in fact all communication in the software development process can be impacted by ambiguity.

Communication problems in software development are not limited to the artifacts produced and the ‘Chinese whispers’ effect along the software development lifecycle. Many inter-team problems are a result of communication issues, and a practical knowledge of the Meta Model can help testers
understand and deal with these issues.

This paper primarily concentrates on artifact analysis with the Meta Model because:

- I am not about to train you to be a therapist, because I am not a therapist.  
- The use of the Meta Model to deal with human belief systems is well covered in the NLP literature.  
- The application to artifacts and software systems is a novel use of the Meta Model, and easily applied by Software Testers.  

**More information on the Meta Model and Software Testing**

Now that I have provided an overview of the Meta Model, I will describe some correspondences with Software Testing.

In much the same way that testing is an open-ended task, there is always another test that could be run. The Meta Model is open-ended in that it is a method of inquiry and each application of the Meta Model elicits another surface structure description of the client’s deep structure. This surface structure description can then be subjected to another Meta Model inquiry.

Just as we do not test for the sake of testing, the Meta Model is not use to get information for the sake of information. Testers get information to effect change in the development team’s beliefs about the system. The Meta Model is used to get information to allow therapists to effect a change in the client.
Testers can ask questions to get information if it seems to them that a transformation has resulted in a communication that may be ambiguous enough to cause problems of understanding.

When I analyse previous testing projects that I have been on from the point of view of the Meta Model, I find that much of the testing use I have derived from the Meta Model is associated with Generalizations and Deletions. I have designed tests specifically to target these two types of
transformations.

Many of the requirements and specification issues that I identify have also associated with Generalization and Deletion, and I have asked simple Meta Model questions to have them clarified. This has resulted in the nature of the testing about to be performed changing and helped achieve better collaboration with other roles involved in the development process.

However, much of the inter-team communication problems have been recognised by distortion.

	Note: Each of the Meta Model elements are not wrong or bad. Depending on the
	context, they may be highly appropriate and exactly right. They can be sources of
	ambiguity, and ambiguity, in software development communication, can be a source of error. 

Copy From http://www.compendiumdev.co.uk/nlp/NLPForTesters(MetaModel).pdf