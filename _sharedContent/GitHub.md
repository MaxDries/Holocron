
## Create a Github Account

1. Go to [https://github.com/join](https://github.com/join).
2. Using your school email address (...@schoolsnet.act.edu.au) create a Github account.

# Jetbrains Tools

For Web Development, using PHPStorm, Webstorm or Pycharm or any other Jetbrains tools, you'll be using the inbuilt Version Control System (VCS). 

![[vcsJetBrainsCommitLink.png]]

## Jetbrains - Cloning the Repository

Open the Jetbrains product you wish to use - Webstorm, Pycharm, PHPStorm etc.

Click “Get from VCS”. 

![[vcsJetbrainsGetFromVCS.png]]

Choose Github in the lefthand side menu.

> [!info] You may need to log in to Github at this stage


Choose the Repository, and the directory you wish to save it locally. 

Choose the repository you created earlier.

Set the folder to an empty folder.  

![[vcsJetbrainsChooseRepo.png]]

Click **Clone.**

![[vcsJetbrainsCloneRepo.png]]

After cloning your repository from Github to the local computer, the project will open.

![[vcsJetbrainsTrust.png]]

> [!info]  If asked, choose to trust the project.


![[vcsJetbrainsRepoCloning.png]]

# Github

## Creating a new Repository - Github

Log into [Github](https://github.com/) (through the browser) and click New for a new repository.

Your screen may look different to this - look for the green "New" button.

![[vcsGitHubNewRepo.png]]

Configure the repository settings.

1. Give the repository an appropriate **name**
2. Add a description
3. Leave as **Public**
4. **Add** a readme file
5. Set the **.gitignore** settings to a setting appropriate for the project type.
    1. This will ignore files that are not needed to synchronise to Github for our project
6. Set the license to MIT.
    
    ![[vcsGitHubRepoSettings.png]]
    
    Click Create Repository.
    
    ![[vcsGitHubRepoCreated.png]]
    

## Github Desktop - Cloning the Repository

> [!info] Github desktop is the GUI interface to clone, commit, push etc repositories to and from Github that runs on your local computer.


Now, open **Github Desktop** on your computer and click "Clone Repository" under the File menu.

![[vcsGitHubDesktopCloneRepo.png]]

Select the new repository you just created.

Choose the path where you want to save it. 

> [!info] The directory will need to be empty.


Click Clone.

![[vcsGitHubDesktopRepoList.png]]

After a short moment, it will then show you this page:

![[vcsGitHubDesktopRepoSelected.png]]

Click on the Current Repository Download in the top left of the window. Right Click on the repository and choose to open it in the file explorer.

![[vcsGitHubDesktopRepoRevealInFinder.png]]

## Github Desktop - Commit and Sync changes

You should see that it's detected the new files. As this is the first time you are committing to the repository, just type in "init" to the summary and detail boxes and then hit commit.

> [!info] When committing later, you'll need to put more details into these boxes. You'll need to explain what changes have been made.


Finally, click "Push Origin" which will synchronise the repository to Github online.

![[vcsGitHubDesktopRepoDiff.png]]

![[vcsGitHubDesktopRepoPushChanges.png]]

# Creating a Pull Request

Now you've made changes to your local project, and committed and pushed the changes to github, you will now want to merge these changes with the original repository. This is done through the process called `Pull Request`.

In Github Desktop, go to the Branch menu and choose Create Pull Request

![[vcsGitHubDesktopCreatePullRequest.png|Screen Shot 2021-08-25 at 2.40.13 pm.png]]

This will take you to [Github.com](http://github.com) and you'll then need to click on the green Create pull Request button.

![[vcsGitHubCreatePullRequest.png|Screen Shot 2021-08-25 at 2.40.18 pm.png]]

This will notify the original owner that you have changes you wish to merge back into the original code repository.

# GitHub Desktop - Update your fork

In Github Desktop, you can update your fork of the repository, but clicking on Update From Main under the branch menu.

![[vcsGitHubDesktopUpdateFork.png]]

**Alternatively**, you can choose to update your fork through [github.com](http://github.com)’s web interface.

![[vcsGitHubSyncFork.png]]