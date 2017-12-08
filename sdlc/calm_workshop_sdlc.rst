*********************************************************
**Calm Workshop – Software Development Lifecycle (SDLC)**
*********************************************************


**SDLC Introduction**
=====================

Understanding the fundamentals of SDLC.  A **software development life cycle** is composed of a number of clearly defined and distinct work phases which are used by systems engineers and systems developers to plan for, design, build, test, and deliver information systems.

|image0|

Successful projects are managed well. In order to manage a project efficiently, the manager or dev team must choose which software development method works best for the project at hand.  All of the numerous software development methodologies that exist are used for different reasons. I've been doing some research to understand why different methodologies exist, and which ones are the most commonly used software development methodologies.

SDLC works by lowering the cost of software development while simultaneously improving quality and shortening production time. SDLC achieves these apparently divergent goals by following a plan that removes the typical pitfalls to software development projects. 

That plan starts by evaluating existing systems for deficiencies. 

- Next, it defines the requirements of the new system. 
- It then creates the software through the stages of design, development, testing, and deployment. 
- By anticipating costly mistakes like failing to ask the end user for suggestions, SLDC can eliminate redundant rework and after-the-fact fixes.

**Stages and Best Practices of SDLC**
-------------------------------------

By Following the best practices and/or stages of SDLC ensures the process works in a smooth, efficient, and productive way.

|image1|

**Identify the current problems. “What don’t we want?”:**

This stage of SDLC means getting input from all stakeholders, including customers, salespeople, industry experts, and programmers. Learn the strengths and weaknesses of the current system with improvement as the goal.

**Plan. “What do we want?”:**

In this stage of SDLC, the team defines the requirements of the new software and determines the cost and resources required. It also details the risks involved and provides sub-plans for softening those risks. In this stage, a Software Requirement Specification document is created.

**Design. “How will we get what we want?”:**

It is the most creative and challenging phase of SDLC.

Occurs in two phases: 

- Logical Design - Specifies user needs. 
- Physical Design - Tells the programmer what the candidate system must do. 

This phase of SDLC starts by turning the software specifications into a design plan called the Design Specification. All stakeholders then review this plan and offer feedback and suggestions. It’s crucial to have a plan for collecting and incorporating stakeholder input into this document. Failure at this stage will almost certainly result in cost overruns at best and total collapse of the project at worst. 

**Build. “Let’s create what we want.”:**

This SDLC stage develops the software by generating all the actual code. If the previous steps have been followed with attention to detail, this is actually the least complicated step.

Less creative then designing phase. 

It is of 3 types: 

- Implementation of a computer system to replace a manual system. 
- Implementation of a new computer system to replace an existing one. 
- Implementation of a modified application to replace existing one on same computer. 

Parallel Runs: In this new system runs with old system which provides assurance, and even helps user staff gain experience. 

**Test. “Did we get what we want?”:**

In this stage, we test for defects and deficiencies. We fix those issues until the product meets the original specifications.

The objective is to determine if the system does what it is designed to do 
Does it support the user as required in an effective and efficient manner 
The review should assess how successful the system is in terms of functionality, performance, and cost versus benefits. 

**Deploy. “Let’s start using what we got.”:**

Often, this part of the SDLC process happens in a limited way at first. Depending on feedback from end users, more adjustments can be made.

**Maintain. “Let’s get this closer to what we want.”:**

The plan almost never turns out perfect when it meets reality. Further, as conditions in the real world change, we need to update and advance the software to match.  The emphasis during this phase is to ensure that needs continue to be met and that the system continues to perform according to specification

Routine hardware and software maintenance and upgrades are performed to ensure effective system operations. 
User training continues during this phase, as needed, to acquaint new users to the system or to introduce new features to current users 

**Note:** The DevOps movement has changed the SDLC in some ways. Developers are now responsible for more and more steps of the entire development process. We also see the value of shifting left. When development and Ops teams use the same toolset to track performance and pin down defects from inception to the retirement of an application, this provides a common language and faster handoffs between teams. APM tools can be used in development, QA, and production. This keeps everyone using the same toolset across the entire development lifecycle.


**Waterfall Development Model**
===============================

Considered the traditional software development method, the waterfall method is a rigid linear model that consists of sequential phases (Requirements, Design, Implementation, Verification, Maintenance) in which distinct goals are accomplished. Each phase must be 100% complete before moving onto the next phase, and traditionally there is no process for going back to modify the project or direction.

|image2|

The linear nature of this method makes it easy to understand and manage. Projects with clear objectives and stable requirements can best use the waterfall method. Less experienced project managers, project teams, and teams whose composition changes frequently may benefit the most from using the waterfall development methodology. However, it is often slow and costly due to the rigid structure and tight controls. These drawbacks led waterfall method users to the explore other development methodologies.

Strengths:

- Easy to understand and use.
- Provides structure to inexperienced staff.
- Milestones are well understood.
- Sets requirements stability.
- Good for manageemnt control (plan, staff, track).
- Worsk well when quality is more important than cost or schedule.

Weaknesses:

- Idealized, doesn't match reality.
- Doesn't reflect itertative nature of exploratory development.
- Unrealistic to expect accurate requirements so early in a project.
- Software is delivered late in project.  Delays bug discovery.
- Difficult to integrate Risk Management.
- Difficult and expensive to make changes to documents - upstream.
- Significant administrative overhead,costly for small teams and projects.

Application:

- Requirements are well understood.
- Product definition is stable.
- Technology is understood.
- New version of an existing product.
- Porting an existing product to a new platform.
- Large projects.

**Spiral Development Model**
============================

The spiral model combines the idea of iterative development with the systematic, controlled aspects of the waterfall model. This Spiral model is a combination of iterative development process model and sequential linear development model i.e. the waterfall model with a very high emphasis on risk analysis. It allows incremental releases of the product or incremental refinement through each iteration around the spiral.

|image3|

The spiral model has four phases. A software project repeatedly passes through these phases in iterations called Spirals.

**Identification:**

This phase starts with gathering the business requirements in the baseline spiral. In the subsequent spirals as the product matures, identification of system requirements, subsystem requirements and unit requirements are all done in this phase.

This phase also includes understanding the system requirements by continuous communication between the customer and the system analyst. At the end of the spiral, the product is deployed in the identified market.

**Design:**

The Design phase starts with the conceptual design in the baseline spiral and involves architectural design, logical design of modules, physical product design and the final design in the subsequent spirals.

**Contruct/Build:**

The Construct phase refers to production of the actual software product at every spiral. In the baseline spiral, when the product is just thought of and the design is being developed a POC (Proof of Concept) is developed in this phase to get customer feedback.

Then in the subsequent spirals with higher clarity on requirements and design details a working model of the software called build is produced with a version number. These builds are sent to the customer for feedback.

**Evaluation and risk Analysis:**

Risk Analysis includes identifying, estimating and monitoring the technical feasibility and management risks, such as schedule slippage and cost overrun. After testing the build, at the end of first iteration, the customer evaluates the software and provides feedback.

The following illustration is a representation of the Spiral Model, listing the activities in each phase.

Based on the customer evaluation, the software development process enters the next iteration and subsequently follows the linear approach to implement the feedback suggested by the customer. The process of iterations along the spiral continues throughout the life of the software.

Strengths:

- Provide early indication of risk without much cost.
- Users see the system early because of rapid prototype tools.
- Critical high-risk functions are developed first.
- Design doesn’t have to be perfect.
- Users can be tied to all lifecycle steps.
- Early and frequent feedback from users.
- Cumulative costs assessed frequently.


Weaknesses:

- Time spent evaluating risks too large for small or low-risk projects.
- Time spent planning, resetting objectives, performing risk analysis and prototyping may be excessive.
- Model is complex.
- Spiral may continue indefinitely .
- Risk assessment expertise is required.
- Developers must be reassigned during non-development phase activities.
- Might be difficult to define objective, verifiable milestones indicating readiness to advance to next iteration.


Application:

- When creation of prototype is appropriate.
- When costs and risk evaluation is important.
- For medium to high-risk projects.
- Long-term project commitment unwise because of potential changes to economic priorities.
- Users are unsure of their needs.
- Requirements are complex.
- New product line.
- Significant changes are expected.


**Iterative Development Model**
===============================

Iterative process starts with a simple implementation of a subset of the software requirements and iteratively enhances the evolving versions until the full system is implemented. At each iteration, design modifications are made and new functional capabilities are added. The basic idea behind this method is to develop a system through repeated cycles (iterative) and in smaller portions at a time (incremental).

The following illustration is a representation of the Iterative and Incremental model

|image4|

Iterative and Incremental development is a combination of both iterative design or iterative method and incremental build model for development. "During software development, more than one iteration of the software development cycle may be in progress at the same time." This process may be described as an "evolutionary acquisition" or "incremental build" approach."

In this incremental model, the whole requirement is divided into various builds. During each iteration, the development module goes through the requirements, design, implementation and testing phases. Each subsequent release of the module adds function to the previous release. The process continues till the complete system is ready as per the requirement.

The key to a successful adoption of an iterative software development lifecycle is rigorous validation of requirements, and verification & testing of each version of the software against those requirements within each cycle of the model. As the software evolves through successive cycles, tests must be repeated and extended to verify each version of the software.

- Starts with a simple implementation of a subset of the software requirements and iteratively enhances the evolving versions until the full system is implemented.
- During each iteration, design modifications are made and new functional capabilities are added.
- Objective is to develop a system through repeated cycles (iterative) and in smaller portions at a time (incremental)


Strengths:

- Some working functionality can be developed quickly early in the lifecycle.
- Parallel development can be planned.
- Results are obtained early and periodically.  Progress can be incrementally measured.
- Less costly when changing scope of requirements.
- Easier to manage risk – High risk items done first.
- Early and frequent feedback from users.
- Issues, challenges and risks identified from each increment can be utilized/applied to the next increment.

Weaknesses:

- May require more resources.
- Management intensive. Management complexity increased.
- End of project may not be known.
- Highly skilled resources are required for risk analysis.
- Risk assessment expertise is required.
- Not suitable for smaller projects.
- Defining increments may require defining complete system.

Application:

Like other SDLC models, Iterative and incremental development has some specific applications in the software industry. This model is most often used in the following scenarios.

- System requirements are clearly defined and understood.
- Some functionality or requested enhancements evolve over time.
- Time to market constraint.
- New technologies being used by the development team.
- Resources with needed skill sets are not available and are planned to be used on contract.
- Some high-risk features and goals may change.


**Agile Development Model**
===========================

The Agile software development life cycle is based upon the iterative and incremental process models, and focuses upon adaptability to changing product requirements and enhancing customer satisfaction through rapid delivery of working product features and client participation. Agile methods primarily focus upon breaking up the entire product into smaller, easily developable, “shippable” product features developed through “incremental” cycles known as “sprints”.

|image5|

Each Agile sprint traditionally lasts from two weeks up to one month. Agile trends now indicate they typically last from seven days up to ten “working” days. Cross-functional teams work simultaneously while developing the product features in daily sprints. The team members are generally experienced and possess varied levels of expertise in activities such as designing, coding, testing, and quality acceptance. At the end of each sprint, a working product feature(s) is developed and presented to the product owner for verification purposes. Once the PO Okays the development, it is presented to the stakeholders, and their opinions are carefully noted to improve upon the current product development cycle. The entire process is repeated through sprints until all the constituent product features are developed.

**Agile software life cycle basics**

An Agile software life cycle is much different as compared to traditional software development frameworks like Waterfall. In Agile, more emphasis is given to sustained and quick development of product features rather than spending more time during the initial project planning, and analysing the actual requirements. The Agile team develops the product through a series of iterative cycles known as sprints. Besides development activity, other aspects pertaining to development such as product analysis, designing the product features, developing the functionality, and “testing” the development for bugs are also carried out during the sprints. The incremental cycles should always produce a “shippable” product release that can be readily deployed.

|image6|

Agile Methods break the product into small incremental builds. These builds are provided in iterations or sprints. Each iteration/sprint typically lasts from about one to three weeks. Every iteration involves cross functional teams working simultaneously in various areas like:

- Planning
- Requirements Analysis
- Design
- Coding
- Unit Testing and
- Acceptance Testing


At the end of the iteration/sprint, a working product is displayed to the customer and important stakeholders.


Agile processes make extensive use of events such as the daily scrum meetings, sprint review meeting, and the sprint retrospective meeting to identify and self-correct the development carried out by the team. Feedback is solicited frequently, as and when needed, to collaborate, and speed up the development process through sharing of ideas and self-management. The feedback system helps to support the self-correction features of Agile frameworks, and is very important.

The roles played in the Agile process constitute of the product owner, scrum master, and the development team. The product owner “owns” the project on behalf of the stakeholders and ensures that the entire project is developed successfully keeping in mind the stakeholders vision of the product as it should “appear” in the market. The scrum master ensures that the Agile process is followed at all times, and does his or her best to resolve any difficulties or technical issues arising during the development process. The team members participate actively in the daily sprints and make sure meaningful and useful development of product features is presented at all times.


**Test Driven Development Model**
=================================

Test-driven development (TDD) is a software development process that relies on the repetition of a very short development cycle: first the developer writes an (initially failing) automated test case that defines a desired improvement or new function, then produces the minimum amount of code to pass that test, and finally refactors the new code to acceptable standards.

The following sequence of steps is generally followed:

- Add a test
- Run all tests and see if the new one fails
- Write some code
- Run tests
- Refactor code
- Repeat

|image7|

The first step is to quickly add a test, basically just enough code to fail.  Next you run your tests, often the complete test suite although for sake of speed you may decide to run only a subset, to ensure that the new test does in fact fail. You then update your functional code to make it pass the new tests. The fourth step is to run your tests again. If they fail you need to update your functional code and retest. Once the tests pass the next step is to start over (you may first need to refactor any duplication out of your design as needed, turning TFD into TDD).

TDD completely turns traditional development around. When you first go to implement a new feature, the first question that you ask is whether the existing design is the best design possible that enables you to implement that functionality. If so, you proceed via a TFD approach.  If not, you refactor it locally to change the portion of the design affected by the new feature, enabling you to add that feature as easy as possible. As a result you will always be improving the quality of your design, thereby making it easier to work with in the future.

Instead of writing functional code first and then your testing code as an afterthought, if you write it at all, you instead write your test code before your functional code.  Furthermore, you do so in very small steps – one test and a small bit of corresponding functional code at a time.  A programmer taking a TDD approach refuses to write a new function until there is first a test that fails because that function isn’t present. In fact, they refuse to add even a single line of code until a test exists for it.  Once the test is in place they then do the work required to ensure that the test suite now passes (your new code may break several existing tests as well as the new one).  This sounds simple in principle, but when you are first learning to take a TDD approach it proves require great discipline because it is easy to “slip” and write functional code without first writing a new test.  One of the advantages of pair programming is that your pair helps you to stay on track.

**There are two levels of TDD:**

- Acceptance TDD (ATDD).  With ATDD you write a single acceptance test, or behavioral specification depending on your preferred terminology, and then just enough production functionality/code to fulfill that test. The goal of ATDD is to specify detailed, executable requirements for your solution on a just in time (JIT) basis. ATDD is also called Behavior Driven Development (BDD).

- Developer TDD. With developer TDD you write a single developer test, sometimes inaccurately referred to as a unit test, and then just enough production code to fulfill that test. The goal of developer TDD is to specify a detailed, executable design for your solution on a JIT basis. Developer TDD is often simply called TDD.

Test-driven development (TDD) is a development technique where you must first write a test that fails before you write new functional code.  TDD is being quickly adopted by agile software developers for development of application source code and is even being adopted by Agile DBAs for database development.  TDD should be seen as complementary to Agile Model Driven Development (AMDD)approaches and the two can and should be used together. TDD does not replace traditional testing, instead it defines a proven way to ensure effective unit testing. A side effect of TDD is that the resulting tests are working examples for invoking the code, thereby providing a working specification for the code. My experience is that TDD works incredibly well in practice and it is something that all software developers should consider adopting.
 


.. |image0| image:: ./media/image1.png
   :width: 3in
   :height: 2in
   
.. |image1| image:: ./media/image8.png
   :width: 4.73125in
   :height: 3.03056in

.. |image2| image:: ./media/image2.png
   :width: 4.73125in
   :height: 3.03056in
   
.. |image3| image:: ./media/image3.png
   :width: 4.73125in
   :height: 3.03056in
   
.. |image4| image:: ./media/image4.png
   :width: 4.73125in
   :height: 3.03056in
   
.. |image5| image:: ./media/image5.png
   :width: 4.73125in
   :height: 3.03056in
   
.. |image6| image:: ./media/image6.png
   :width: 4.73125in
   :height: 3.03056in
   
.. |image7| image:: ./media/image7.png
   :width: 4.73125in
   :height: 3.03056in




