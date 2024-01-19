# Merge Request Process

A good merge request process - that everyone adheres to - is critical to a well-functioning software development team. 

## Gitflow

[Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) is a merge request process/branching strategy that has gained traction over several years. While other, less strict Git strategies have emerged since Gitflow initially came on the scene, Gitflow remains an excellent balance of organization, approval, and CI/CD integration.

!!! note
    There are many good visualizations of Gitflow on the internet, [including from Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow). It's an important concept to have locked in, so take the time to research it, if necessary.

## How It Works

The goal of Gitflow is to make sure commits make their way through from developer environment all the way to production, with a minimum of conflicts and risk of errors or substandard code. In so doing, several different branches are utilized to keep concerns separate.

- `master` - The "main" branch. Releases are generated at a regular interval from `master`. `master` will, in general, only receive commits from `develop`.
  - The exception is if there needs to be a hotfix or some other off-cycle release. Then a commit may be made from a feature branch into `master`, but it goes without saying that this is to be avoided whenever possible.
- `develop` - The integration branch for features, bugs, and chores. `develop` receives commits from these individual feature branches. These commits are then collectively merged into `master`.
- Feature branches
  - `feature` - A branch that contains new functionality in one way or another supporting the end user experience. Named in association with the JIRA ticket, following the pattern of `feature/XXXX-1234 - Ticket description goes here`.
  - `bug` - A branch that addresses an error or failure that is impacting the end user experience. Named in association with the JIRA ticket, following the pattern of `bug/XXXX-1234 - Ticket description goes here`. When a bug is ready to be merged, it must be merged in to both `master` and `develop` to keep these branches in sync and avoid conflicts down the line.
  - `chore` - A branch that addresses tech debt, or some other maintenance task that does not actually impact the end user experience. Named in association with the JIRA ticket, following the pattern of `chore/XXXX-1234 - Ticket description goes here`.

## Approvals

Use the Gitlab interface to create a merge request, making sure to follow the steps in the merge request template to verify the code is ready for `develop`. Then, select a project "maintainer" as a Reviewer, and select a peer as an Assignee. Both of these individuals must approve your code before it can be merged into `develop`.

!!! note
    Merge Requests can be stressful, but be confident! Firstly, be confident when you are giving feedback; you have good ideas and know what you're doing - otherwise, you wouldn't be in this position! If you see something that could be refactored or doesn't make sense, say something. Don't assume that it is correct, even if the person you're reviewing for is more senior than you. You are both more likely to come out of a Merge Request process as better developers if you say something, whereas you'll neither one benefit if you just rubber stamp everything.

## Releases

Releases are generated off `master`, at a cadence to be determined on a team-by-team basis.