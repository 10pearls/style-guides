# Git Source Control Management

### Table of contents
1.	[Branches](#branches)
2.	[Pull Requests](#pull-requests)

#### Branches
A project should at least have the following branches
- [master](#master)
- [staging](#staging)
- [develop](#develop)
- [feature](#feature)


##### master
`master` will always be considered as Production/Release branch. Developers should never commit their work directly on the `master` branch.

##### staging
`staging` branch will host source deployed on the `staging` server, rest of the process will remain same as master. At the point of release, `staging` branch will be forwarded to `master` branch.

##### develop
`develop` branch is the main point of reference for developers and will host the source deployed on QA server. All feature branches must be branched out from `develop`. It's the responsibility of the developer to keep his feature branch up to date with develop. Developers should never commit directly on the `develop` branch instead they should submit a Pull Request to merge their feature branch into `develop`.

**Note:** It is highly recommended that `develop` should be the default branch of the repository to reduce errornous commits on `master`

##### feature
Each feature branch belongs to one of the following and should contain the suggested naming convention:

| Branch type  | Naming convention |
| ------------- | ------------- |
| user story  | feat/XXXX  |
| bug  | bug/XXXX  |
| hotfix  | hotfix/XXXX  |
| patch  | patch/XXXX  |

The naming convention can vary in teams but should be kept consistent across the team. 

**Note:** Prior to pushing the feature branch and submitting a [pull request](https://help.github.com/articles/using-pull-requests/), developer must update his branch from `develop`. Updates can be done via [merge](#merge) or [rebase](#rebase) which will be discussed below.

#### Pull Requests
All branches should be merged via Pull Requests. Following links are guides to creating Pull Requests in some tools
- [Github](https://help.github.com/articles/using-pull-requests/) 
- [Bitbucket](https://www.atlassian.com/git/tutorials/making-a-pull-request/)
- [Gitlab](https://about.gitlab.com/2014/09/29/gitlab-flow/) (scroll to find Merge/Pull Request heading)

##### Pull Request should:
- contain a single feature/bug/hotfix
- have a title which is short and descriptive
- contain an elaborate description of the problem being addressed. If you are using another Project Management tool like JIRA, just place the links to the JIRA items in markdown
- provide an assignee Github user

##### Reviewers of Pull Requests should:
- throughly go through the changes suggested and identify logical pitfalls, code smells and standards
- make appropriate inline comments via `@mentions` to notify developers and help them make changes in an intuitive way
- merge the pull request when satisfied and close off the branch (closing branch is important)
