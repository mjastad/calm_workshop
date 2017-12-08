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

Identify the current problems. “What don’t we want?”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This stage of SDLC means getting input from all stakeholders, including customers, salespeople, industry experts, and programmers. Learn the strengths and weaknesses of the current system with improvement as the goal.

Plan. “What do we want?”
^^^^^^^^^^^^^^^^^^^^^^^^

In this stage of SDLC, the team defines the requirements of the new software and determines the cost and resources required. It also details the risks involved and provides sub-plans for softening those risks. In this stage, a Software Requirement Specification document is created.

Design. “How will we get what we want?”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is the most creative and challenging phase of SDLC.

Occurs in two phases: 

- Logical Design - Specifies user needs. 
- Physical Design - Tells the programmer what the candidate system must do. 

This phase of SDLC starts by turning the software specifications into a design plan called the Design Specification. All stakeholders then review this plan and offer feedback and suggestions. It’s crucial to have a plan for collecting and incorporating stakeholder input into this document. Failure at this stage will almost certainly result in cost overruns at best and total collapse of the project at worst. 

Build. “Let’s create what we want.”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This SDLC stage develops the software by generating all the actual code. If the previous steps have been followed with attention to detail, this is actually the least complicated step.

Less creative then designing phase. 

It is of 3 types: 

- Implementation of a computer system to replace a manual system. 
- Implementation of a new computer system to replace an existing one. 
- Implementation of a modified application to replace existing one on same computer. 

Parallel Runs: In this new system runs with old system which provides assurance, and even helps user staff gain experience. 

Test. “Did we get what we want?”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this stage, we test for defects and deficiencies. We fix those issues until the product meets the original specifications.

The objective is to determine if the system does what it is designed to do 
Does it support the user as required in an effective and efficient manner 
The review should assess how successful the system is in terms of functionality, performance, and cost versus benefits. 

Deploy. “Let’s start using what we got.”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Often, this part of the SDLC process happens in a limited way at first. Depending on feedback from end users, more adjustments can be made.

Maintain. “Let’s get this closer to what we want.”
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

- Model based on division of work.


**Test Driven Development Model**
=================================

- Model based on development around test acceptance.



.. |image0| image:: ./media/image1.png
   :width: 3in
   :height: 2in
   
.. |image1| image:: ./media/image8.png
   :width: 4.73125in
   :height: 3.03056in

.. |image2| image:: ./media/image2.png
   :width: 4.73125in
   :height: 3.03056in
   
.. |image4| image:: ./media/image4.png
   :width: 4.73125in
   :height: 3.03056in

