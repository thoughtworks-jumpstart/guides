# Continuous Integration & Continuous Deployment

## Content

1. Definition of Done(DOD)
2. Continuous Integration(CI)
3. Continuous Development(CD)

## Definition of Done

An agreement on what needs to be completed before a story is fully completed and ready to be ship to the user. The agreement is set by all state holders(Developers(Dev), Quality Analyst(QA), Business Analyst(BA), Product Owner(PO)).

### benefit

1. DOD gives everyone in the team the same expectations on what is included and what is not in a user story.
2. Gives team more clarity during estimations on how much a team can likely complete
3. Setting the right expectations for PO, why features might take longer than the time to get barely working code.
4. Prevent rework and arguements why certain things were not covered.

### Common DOD incluces but not limited to

1. All acceptance criteria(AC) is completed
2. Code does not break any linting rules
3. Unit test coverage higher than a certain percentage
4. Code have been viewed by at least 1 other member
5. All behaviors and edge cases are tested by QA
6. The PO understands the scope of the changes and have verified

### Side Note:

1. DOD should contain things that are only really required by the story
2. Not all stories will have the same DOD
3. A common misbelief that DOD is only for the developer. This is definetely not true.
   For example

- User story should be clearly written(remember to INVEST). PO should be available to clarify and verify any requirements.
- QA can help to identify which part of the code causes issue.
- QA should take ownership of the test and work closely with developers.
- BA or PO can also work on task that extract data out of the database.

## Continous Integration

A software development practice where members of a team integrate their work frequently, usually each person integrates at least daily - leading to multiple integrations per day. Each integration is verified by an automated build (including test) to detect integration errors as quickly as possible. - https://martinfowler.com/

### Benefits

1. Prevents developers changing so much code from the main branch that it becomes difficult and expensive to combine the code(This difficulty, costly and highly likely to cause migraine process is also known as "Merge Hell").
2. The reduce in integration problem allows team to move more smoothly.
3. When your teammate integrate code in a small chuck, it allows you to foresee how your code can integrate with what your colleagues are working and bring important discussion on problems much earlier in the development process.

### How it is done

1. Find the smallest logical requirement.
2. Develop the requirement by repeatedly writing test, new code and apply refactoring.
3. Run all the test locally and make sure everything pass
4. Integrate the code by pushing to the repo
5. An automated build gets triggered and run all test again, with the setup as close to production as posible.
6. Monitor the build, if the build fails, proritise to fix the build immediately and get as much help as required(other team member should also proritise to fix the build if asked to help). If the fix will take time, revert the code to a working state, fix the changes then repeat.
