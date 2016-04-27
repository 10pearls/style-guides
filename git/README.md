# Git Source Control Management

### Table of contents
1.	[Branches](#branches)

#### Branches
A project should at least have the following branches
- [master](#master)
- [staging](#staging)
- [develop](#develop)
- [feature](#feature)


##### master
Master will always be considered as Production/Release branch. Developers should never commit their work directly on the master branch.

##### staging
Staging branch will host source deployed on the staging server, rest of the process will remain same as master. At the point of release, staging branch will be forwarded to master branch.

##### develop
Develop branch is the main point of reference for developers and will host the source deployed on QA server. All feature branches must be branched out from develop. It's the responsibility of the developer to keep his feature branch up to date with develop. Developers should never commit directly on the develop branch instead they should submit a Pull Request to merge their feature branch into develop.

##### feature
Each feature branch belongs to one of the following and should contain the suggested naming convention:

| Branch type  | Naming convention |
| ------------- | ------------- |
| user story  | feat/XXXX  |
| issue  | iss/XXXX  |
| hotfix  | hotfix/XXXX  |
| patch  | patch/XXXX  |

The naming convention can vary in teams but should be kept consistent across the team. 

**Note:** Prior to pushing the feature branch and submitting a [pull request](https://help.github.com/articles/using-pull-requests/), developer must update his branch from develop. Updates can be done via [merge](#merge) or [rebase](#rebase) which will be discussed below.
