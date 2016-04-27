# Git Source Control Management

### Table of contents
1.	[Branches](#branches)

#### Branches
A project should at least have the following branches
	- master
	- staging
	- develop
	- feature

##### master
Master will always be considered as Production/Release branch. Developers should never commit their work directly on the master branch.

##### staging
Staging branch will host source deployed on the staging server, rest of the process will remain same as master. At the point of release, staging branch will be forwarded to master branch.

##### develop
Develop branch is the main point of reference for developers and will host the source deployed on QA server. All feature branches must be branched out from develop. It's the responsibility of the developer to keep his feature branch up to date with develop. Developers should never commit directly on the develop branch instead they should submit a Pull Request to merge their feature branch into develop.

##### feature
Each feature branch belongs to one of the following:
 	-	user story
 	-	issue
 	-	hotfix
 	-	patch

Each should be branched off from develop with a naming convention such as:
 	-	feat/XXXX
 	-	iss/XXXX
 	-	hotfix/XXXX
 	-	patch/XXXX

 The naming convention can vary in teams but should be kept consistent across the team.
