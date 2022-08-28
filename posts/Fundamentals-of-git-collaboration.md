---
title: 'Fundamentals of git collaboration'
date: 'August 28, 2022'
excerpt: 'Fundamentals of git collaboration'
cover_image: '/images/posts/gitlab-logo-gray-rgb.png'
---

# Fundamentals of git collaboration. 

## Common pitfalls.

### :large_orange_diamond: Untracked pulls.
Consider the case of two users (user1 and user2) working on the same file. Firstly, user1 makes a change and commits it to the central repository, and then user2 also makes a change, but before committing to the central repository, user2 updates the new version by pulling it. However, git warns user2 that pulling would overwrite untracked changes.  

<img src="https://user-images.githubusercontent.com/34083808/186171457-18b290a7-e8ff-4e10-8e5a-092b08ff1332.png" alt="drawing" width="1000"/>

In this case, you either clean the repo using ``` git reset --hard``` or commit the change and then perform the Pull command.

```
git commit --am "User 2 change"

git pull origin master
```

after you pull new source code, git will warm you the merge conflic and you need to resolve conflict and then commit again. 

<img src="https://user-images.githubusercontent.com/34083808/186173890-2a9382a7-6c59-4330-9785-e3348efefa88.png" alt="drawing" width="400"/>

<img src="https://user-images.githubusercontent.com/34083808/186174020-9f657abc-4573-42d6-af16-b56219452ade.png" alt="drawing" width="400"/>

### :large_orange_diamond: Forced Pushes.
When you commit and push to the central repositories, git may reject your change because another user changed the file.  In this case, if you perform a push command with ```--force/-f``` flag it will force to overwrite your change to the central repositories and your colleague's change will accidentally disappear.
```
# It is very dangerous to use this command without being aware of its dangers
git push origin master --force
```

## Best practies.

### :large_orange_diamond: Committing and syncing. 
- Commit early, commit often.
- Pull frequently from upstream / fetch. 

### :large_orange_diamond: gitignore.
The GitIgnore file prevents unneeded files from entering your repository. 

``` gitignore
# .gitignore

.<filename>
# without back flag, git will ignore both file and folder with that name.
.<foldename>/
*.<extension>
# wrap sub directory
**<folder/name>
```
Please check [this](https://www.atlassian.com/git/tutorials/saving-changes/gitignore) Document for more detail of the *.gitignore format file. 

## Standardize line endings with autocrlf.
The problem occurs if we write source code on different systems such as Unix and Windows. The Windows machine supports both line feed (LF) and carriage return (CR), while the Unix machine only supports LF.

<img src="https://user-images.githubusercontent.com/34083808/186182523-83873164-602f-46f1-95fe-b1bb689ea617.png" alt="drawing" width="800"/>

This can be resolved by using the option ``` core.autocrlf``` 

```
git config core.autocrlf input
```
> autocrlf Setting values. 

<img src="https://user-images.githubusercontent.com/34083808/186184266-0f6484b4-7d6f-4bd3-b419-4a9a0cfa6cb9.png" alt="drawing" width="800"/>

## Branch Namming. 

structure branh name will make benefit. Keeping branch name short and consistent and informative. below was some example of branch name. 
```
git branch -b bug/123/menu-issue

git branch -b feat/123/new-email-feature

# type of change/ change number/ Short Name
```

## Write Descriptive commit messages.
You may be familiar with the ```git commit -m " message "``` command, which includes a poor commit message and is not recommended for team collaborations. 

To write better commit message, we show write the message by the editor instead of command line, to setup the editor for the command you can use the below command. 
```
git config --global core.editor "<path to our favories editor>"
```

Listed below is a table of popular editors and matching git config commands:
| Editor      | config command |
| ----------- | ----------- |
|Atom	|~ git config --global core.editor "atom --wait"~|
|emacs	|~ git config --global core.editor "emacs"~|
|nano	|~ git config --global core.editor "nano -w"~|
|vim	|~ git config --global core.editor "vim"~|
|Sublime Text (Mac)	|~ git config --global core.editor "subl -n -w"~|
|Sublime Text (Win, 32-bit install)	|~ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublimetext.exe' -w"~|
|Sublime Text (Win, 64-bit install)	|~ git config --global core.editor "'c:/program files/sublime text 3/sublimetext.exe' -w"~|
|Textmate	~ git config |--global core.editor "mate -w"~|


after writing the message and save then close the editor. Git automatically wrap that message to the commit message. <br>
<img src="https://user-images.githubusercontent.com/34083808/186295489-aafe0ccf-2e2b-4c95-8d51-cd2ee8919fc3.png" alt="drawing" width="600"/>

> We can also add template message to support write robust messages

```
git config commit.template "template path"
```

## Team Composition and member. 

Team member role. <br>
<img src="https://user-images.githubusercontent.com/34083808/186905026-75188267-a059-4a82-9c19-3f90a9b9e492.png" alt="drawing" width="800"/>

Git was make for team with diferent size. The size of the team will decide how we work with git, there are several model adapt with team size. 

### Small size Dev team. 
every member of the team will act as the maintainer. They can have full access of read and write to central repository. <br>

<img src="https://user-images.githubusercontent.com/34083808/186906973-90d2c4b5-ccfa-4a6f-8a5f-56a6b4f4776d.png" alt="drawing" width="400"/>

### Normal size Dev team. <br>

You will have at least one maintainer and the remain team members will be acted like the contributor to the project. They cannot have the access to main repo and need to create the branch and merge request to the maintainer. 

<img src="https://user-images.githubusercontent.com/34083808/186907632-938ef38e-b558-45a4-941a-096c3c9a8359.png" alt="drawing" width="600"/>

### Large size Dev team. <br>

With the large team, they can devided into sub repository and have several maintainers mannage sub repository. 

<img src="https://user-images.githubusercontent.com/34083808/186908118-73d5060d-24d4-4b05-8259-0f627fa7a9ef.png" alt="drawing" width="800"/>

> It better to make document to decribe about the role of maintainers and contributor. we can refer to [zendframwork](https://github.com/zendframework/maintainers) which incude great document of how to work in the community.

# Team with Remote Platform
## Central Repository.
The place used by teams for central storage of souce code. It's self or cloud hosted. We will look at several option for hosting. 
- The first one, simply install git into your hosting enviroment and set up a new repository with the command.
```
# init the repo on the remote server
git init --bare
```
- Some team use Repository manager on Cloud based service or on-premises installation. It will include the capabilities of Issue tracking, pull request and code reviews, access controls, inline editing.  

let check the three most popular Repository manager availabe.

<img src="https://user-images.githubusercontent.com/34083808/186919030-96f7e2bb-ffba-423e-a549-c55bf67a4b2e.png" alt="drawing" width="800"/>

In this course, We will use Gitlab as the remote server. Please refer to the install gitlab gist to install gitlab server. 

After install gitlab server, you need to create root password and start create repository and new account. 

<img src="https://user-images.githubusercontent.com/34083808/187016043-2f1e92a5-6a38-4e73-a8b8-35cbef452b4d.png" alt="drawing" width="800"/>

after create new account for maintainer and contributor, you can add them to your created project by go to the setting of project and invite them to you project. 

<img src="https://user-images.githubusercontent.com/34083808/187016625-39b5a0a8-22e9-4c62-a5ed-9d34b2c9a855.png" alt="drawing" width="800"/>


## Git Branching Strategies

Simply put, a branching strategy is something a software development team uses when interacting with a version control system for writing and managing code. As the name suggests, the branching strategy focuses on how branches are used in the development process

<img src="https://user-images.githubusercontent.com/34083808/187017584-ef8e26c5-2533-424e-82a0-5a93b54eb52a.png" alt="drawing" width="600"/>

### Long running Branches. 
Long running branches alway remain open, storing the history of a particular line of development. Often, a master or trunk branch holds the code for the current release of the project. Another long running branch is develop, which allow for intergation of changes in process. 

- Branches that remain open and receive regular merges
- Often correspond with development phases. 

<img src="https://user-images.githubusercontent.com/34083808/187017820-b55db473-a6e9-4509-86de-26dc39791592.png" alt="drawing" width="600"/>

### Feature branches. 
- Branches created for a particular feature. 
- Stores work related to the feature for a short time. 

<img src="https://user-images.githubusercontent.com/34083808/187017869-ee81adc1-1a24-4751-97f9-28c969a76c23.png" alt="drawing" width="600"/>

### Hotfix Branches 

- Branches create to quickly apply a patch to a production environment. 
- Hotfixes are also merged into other long-running branches. 

<img src="https://user-images.githubusercontent.com/34083808/187017907-33c1c05e-1af1-45c8-a2e5-11d11010f60b.png" alt="drawing" width="600"/>


## Git Workflows

- Define a strategy for commiting, merging, and promoting changes to a repository. 
- Structured delivery of work, increasing efficiencies and reducing errors. 
- Combines various branching strategies
- Introduces oppotunities for code reviews and protected branches. 

There are several worklows available, will will discuss about trunk-based development and git flow in following section. 

## Trunk-based development. 

Trunk-based development (TBD): updates and pull occur from a single branch named trunks. Thunk will be the only long live branch for pulling, merging. some advatage of TBD are redude the problem frequently occur when merging long-lived branches such as breaking the build and duplicate work. 

<img src = "https://user-images.githubusercontent.com/34083808/187018142-e0bf5d20-befe-431c-88c6-285bc9e40979.png" alt= "drawing" width=600 />

> Trunk-based development best practies. 
- Small commits on daily basic. 
- Organize work into small tasks.
- Tightly intergrated with CI/CD practices. If test failed, all team member need to drop everything  and try to fix it. 
- Release ready. 

Ref: [Trunk-based development introduction](https://trunkbaseddevelopment.com/)

## Git Flow
Git flow is a popular workflow for git, That center around two long-lived banches Master and Develop. Master branch contain a copy of current production code and it's titly control. 

<img src = "https://user-images.githubusercontent.com/34083808/187021381-d88ca346-bc22-4e9e-b252-d8799e031258.png" alt= "drawing" width=600 />

Develop is the parent branch for feature branches, it used by everyone to build feature branches. 

<img src = "https://user-images.githubusercontent.com/34083808/187021229-b968e21b-cc35-4622-9885-eb28fd687907.png" alt= "drawing" width=600 />

<img src = "https://user-images.githubusercontent.com/34083808/187021335-e8963812-0699-47d9-90d5-0207f371579f.png" alt= "drawing" width=600 />

Once, feature branch is completed, this branch then merge back to develop branch. if the develop branch collect enough feature for new release. Release branch is created to release new feature. In the release branch, bug fix can be make in parallel to new work being perfomed in develop. <br> 

The release branch then merged into master branch and develop  branch after the code is ready for production.

<img src = "https://user-images.githubusercontent.com/34083808/187021528-83467849-8be6-4f1d-97a5-252656eefdfa.png" alt= "drawing" width=600 /> <br>
Git flow allows Hotfix branches create from master branch for bug happen on production. 
<img src = "https://user-images.githubusercontent.com/34083808/187021575-b64ee22c-07b6-47b2-ab0e-f7937a5de3c0.png" alt= "drawing" width=600 />

### Benefits of git flow. 
- Workflow is based around releases. 
- Allows for parallel development by leveraging short-live branches for features, hot fixed and releases. 
- Pull/merge requests used for updates to master and develop. 

### Git Flow Chart

![image](https://user-images.githubusercontent.com/34083808/187024915-6ee01964-b091-47bf-93bf-88f23223d29d.png)

### Difference between TBD and gitflow. 
| Trunk Based      | Git Flow |
| ----------- | ----------- |
| Single long-live branch      | Multiple branches/type       |
| Work is alway release ready  | Master branch is release ready        |
| Compliments continuous deployment  | Compliments release        |

## Protecting branches. 
go to your repository -> setting -> repository ->Protected Branches <br>
<img src = "https://user-images.githubusercontent.com/34083808/187022365-6872ef56-ceed-48bd-84ff-88b96433eee1.png" alt= "drawing" width=600 />

## issues
Issues serve as the seeds for new work items. You can create new issue by clicking on create new issue and fill out some information. 

<img src = "https://user-images.githubusercontent.com/34083808/187022731-b2115d04-3777-447f-9589-66a08ba8e018.png" alt= "drawing" width=600 />

After that you can go to issue Board, the new created issue will appear on backlogs. You can create project lable and add the issue from backlogs to project label for easy management. 

<img src = "https://user-images.githubusercontent.com/34083808/187022865-18610b6b-0a40-456f-a74a-e8912e1b0b00.png" alt= "drawing" width=600 />

The Milestone can be use to group the issues together to track big size of items. 

<img src = "https://user-images.githubusercontent.com/34083808/187022967-37567e83-b3f4-45a6-9115-1733168fb829.png" alt= "drawing" width=800 />

## Feature Branches.

We will create new feature branch for the issue which made by maintainer. At first, go to the issue click on create mergerequest pull down and click on create new branch. 

<img src = "https://user-images.githubusercontent.com/34083808/187023168-3735028c-3170-4fc4-ad44-8a9a6e268515.png" alt= "drawing" width=400 />

after fixing and push to central repository, create merge request to maintainer. 

<img src = "https://user-images.githubusercontent.com/34083808/187023748-0e97e27a-b9f3-4342-9e04-b141fb00c876.png" alt= "drawing" width=600 />

Now witching back to maintainer role, we check the merge request and perfome code review for this merge request. If there is problem happen, you can comment dirrectly on code and request to contributor to fix incorect code. 

<img src = "https://user-images.githubusercontent.com/34083808/187023892-8c6f359d-0e25-4dd0-a437-0729dbf499b6.png" alt= "drawing" width=600 />

after fixing the feedback from the maintainer, we push the change to gitlab and then comment our change to merge request. The next step is merge feature branches. 


## Merging feature branches. 

after confirm everything right, maintainer going to merge new feature to develop branch. 

<img src = "https://user-images.githubusercontent.com/34083808/187024149-7140792d-7375-4da5-9f64-bf77bb53119d.png" alt= "drawing" width=600 />

next step is create release branch for new release. 

<img src = "https://user-images.githubusercontent.com/34083808/187024261-2127a4a0-3fb9-4773-bd3c-f89e1e8bbfa1.png" alt= "drawing" width=600 />

At this point, we need to create some QA testing. If you find any problem/bug within release branch, you can make fix and commit to release branch. After all has been fixed, then the final merge request to production is make. 

> note: after production is make, you need to merge all change make at release branch back to develop branch. 


Ref: 
- [devops-branching-strategies](https://www.bmc.com/blogs/devops-branching-strategies/)
- [What Are the Best Git Branching Strategies](https://www.flagship.io/git-branching-strategies/#:~:text=A%20branching%20strategy%2C%20therefore%2C%20is,interact%20with%20a%20shared%20codebase.)
- [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- [git-flow cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/)

