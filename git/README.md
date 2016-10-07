# Git Source Control Management

### Table of contents
1.	[Branches](#branches)
2.	[Pull Requests](#pull-requests)
3.	[Commit Messages](#commit-messages)

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

#### Commit Messages
Each commit messages should be in the following format:

```xml
<header><type>(<scope>): <subject><header>
<BLANK-LINE>
<body>
<BLANK-LINE>
<footer>
```
*Any line of the commit message cannot be longer 100 characters! This allows the message to be easier to read on GitHub as well as in various git tools.*

##### Header
**Mandatory** field describing the type, scope and subject for the commit.

##### Revert
If the commit reverts a previous commit, it should begin with `revert:`, followed by the header of the reverted commit. In the body it should say: `This reverts commit <hash>.`, where the hash is the SHA of the commit being reverted.

##### Type
Must be one of the following:

* **feat**: A new feature
* **fix**: A bug fix
* **docs**: Documentation only changes
* **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing
  semi-colons, etc)
* **refactor**: A code change that neither fixes a bug nor adds a feature
* **perf**: A code change that improves performance
* **test**: Adding missing tests
* **chore**: Changes to the build process or auxiliary tools and libraries such as documentation
  generation

##### Scope
The scope could be anything specifying place of the commit change. For example `$location`,
`$browser`, `$compile`, `$rootScope`, `ngHref`, `ngClick`, `ngView`, etc...

##### Subject
The subject contains succinct description of the change:

* use the imperative, present tense: "change" not "changed" nor "changes"
* don't capitalize first letter
* no dot (.) at the end

##### Body
Just as in the **subject**, use the imperative, present tense: "change" not "changed" nor "changes".
The body should include the motivation for the change and contrast this with previous behavior.

##### Footer
The footer should contain any information about **Breaking Changes** and is also the place to
reference GitHub issues that this commit **Closes**.

**Breaking Changes** should start with the word `BREAKING CHANGE:` with a space or two newlines. The rest of the commit message is then used for this.

##### Commitizen
The above workflow can be automated through [Commitizen](https://github.com/commitizen/cz-cli) command line interface.
