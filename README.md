# Assignment 1: Agile testing

## Introduction and general approach

*Remark: by ticket I mean scrum item like epic, story, bug etc.*

As the single QA Engineer I would start by a meeting with PO and senior developers to determine:</br>
- key software functionalities (core and most used)</br>
- functionalities most vulnerable to regressions</br>
- division of tasks between scrum teams</br>
  - do they develop the same product</br>
  - if yes: do they work feature-wise?</br>
    - if yes: how are intregrations / intersecting features are being tested?</br>
- developers approach during testing</br>
  - do they cover only happy paths?</br>
  - do they perform negative scenarios?</br>
  - are there any unit tests?</br>
    - if yes: are new features being covered simultaneously?

In a perfect world, no story/defect should be marked as done with no QA testing. But of course, that's not possible with only one tester. </br>
I'd propose the following 'testing pyramid' based on the priority of QA action (from the highest): </br>
- new features (stories) in core / vulnerable to regressions functionalities</br>
- high priority customer issues</br>
- new features (stories) in existing functionalities, but not core / vulnerable to regressions</br>
- db migrations</br>
- low priority customer issues</br>
- new features (stories) independent of existing ones</br>
- internal bugfixes

If possible at least top 3 ticket types should be tested by QA team. The others based on QA's capacity. If no capacity we follow the old approach: developers perform testing, but developer should not test their own code -> ask peers for testing to keep at least a minimum level of independent testing.

If there are no developers unit tests, I would strongly advise starting writing them. Especially when there are no QA automation tests yet. From my perspective no story should be moved to 'Testing' without adding new tests. It would be nice to have pipelines for automatic unit test execution. So a feature branch should be tested against existing & new unit tests automatically. And the same should happen after merging a branch.

Simultaneously to daily scrum activities & testing I'd book some slots for PoC of a new QA automation framework. When PoC meets expectation of stakeholders, smoke scope should be automated as soon as possible (also by booking part of QA time on automation tasks during sprints). Automated smoke scope should become a basic ovierview of quality. Job with tests should be executed daily to follow the CI/CD approach. At some point story points should include time needed for QA automation. But first old features require automation.


## How would you work with your team members in the activities they do in the sprint?

When participating daily stand-ups I'd try to give developers a hand during ticket testing with no QA assigned. If possible and needed, I could quickly join to ticket testing and perform some exploratory testing based on my experience. If needed I'd share good practices about writing unit tests and their coverage as well as manual testing. </br>
Once upon a time when smoke tests are automated, I could inform a team and give a status about daily execution. </br>
For transparency I'd suggest adding a new column to scrum board: **Testing failed**. Thanks to that, team members could easily identify functionalities that could not work as expected, but are already introduced to test environemnt.</br>
I'd actively participate in retrospective meetings to improve the testing process and discover gaps.


## How would you want to be involved in the different phases of the Scrum process to ensure quality?

Starting from sprint planning we should divide sprint backlog into 2 'virtual' parts: </br>
- QA assignment needed for testing</br>
- can be tested by developers</br>

During daily stand-ups that division should be easily monitored & adjusted depends on devs & QA capacity.</br>
Also in the middle of sprint it'd be nice to have a backlog refinement session to ensure the backlog contains the appropriate items prioritized not only by stakeholders requests, but also by team availability and capacity. </br>
On a daily basis I focus on  performing the following activities: </br>
- testing tickets assigned to me (as agreed during planning)</br>
- covering those tickets in automation scope if they are crucial</br>
- when no tickets assigned:</br>
  - adding existing features to automated scope by their priority</br>
  - supporting developers in testing on demand</br>


## How would you optimize the release cycle, including the "hardening" sprint?

First of all, I'd add a new field in tickets - **Fix version**.</br>
Every ticket ready for testing (when feature branch is merged) should be updated with "Fix version" value, e.g. "release_v1.01".</br>
Then I'd create a new board based on the current release version ("Fix version" == current release). Thanks to that, it would be easy to monitor the release scope and tickets status.</br>

Secondly, I'd start using service tagging and maintaining product config.</br>
Let's say that our product is built from the following layers (all kept in separate repositories):</br>
- db</br>
- UI platform</br>
- API service 'A'</br>
- API service 'B'</br>
- ...</br>
- API service 'Z'</br>

Before the release if there are no blocking bugs related to service 'X', *master/develop* branch should be cut & serviced tag created. If service 'X' has blocking defects then the service should not be released. And all tickets related to this particular service should be moved to the next release by updating "Fix version".</br>
Based on all tags we should maintain 3 version configs being used during instance/environment deployment:</br>
- test - all tags set to latest</br>
- production - services in tags released to production</br>
- release - tags from production updated with new tags for services under release process</br>

So we could release only tagged, ready services. Not all the product at once. So it would be easier in terms of failed tickets - no rollbacks needed.</br>

Based on the previous ideas. At the beginning of 4th sprint it would be nice to have the release planning meeting in order to:</br>
- review what was merged during past 3 sprints</br>
- what is not yet passing tests</br>
- are any rollback needed</br>
- review all tickets with no "Fix version", but in *Testing*/*Failed Testing* status</br>

Testing activities should be performed in a dedicated environment with release candidates, free of untested tickets and unwanted merges. Against release environment we could run automed tests from QA framework, as well as perform e2e testing based on release content focusing on component integration if needed.</br>
And I'd distinguish 2 **Done** statuses in tickets:</br>
- Ready for Release (before release)</br>
- Complete (after release)</br>

That could make the process and tickets more understandable and transparent.</br>
Perhaps some day the sprint 4th will no longer be the "hardening" sprint, but the usual one with extra release activities for only 2 members: Release Manager and QA Owner.
