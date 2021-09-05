## Linking Android Studio and GitHub

An FTC team that uses Android Studio for robot programming should also use cloud storage for backup, and to facilitate file sharing.
GitHub is the most popular (but not the only) choice.

Android Studio and GitHub play well together!  There are features provided in Android Studio that make GitHub integration easy.
But you have to establish authentication credentials to connect an Android Studio project to a GitHub account, and remote repository.
Until recently, you could use the familiar Account Name and Account Password pair to authenticate.  But that’s no longer possible.
To improve security, GitHub now requires a different method for computer applications to authenticate to their accounts.
The easy-to-remember Name and Password is a thing of the past.
(This change took effect on August 13, 2021.  Some returning FTC programmers are in for a surprise.)

Individual developers and programmers usually can manage their authentication needs with a Git Credentials Manager add-on program.
This may not work well for an FTC team, where several programmers may be using classroom computers,
from a pool shared between teams, or their own personally-owned computers - or both.
Many teams are part of a larger sponsoring organization, such as a school;
there are some advantages to using a workflow and process common to all teams sponsored by the same organization.

We at the Princeton STEM Academy would like to share the authentication workflow we use with the FTC community.
We hesitate to call it a “best practice” - but it works for us, and it may work for your teams.

Pros:
- Works for a pool of shared classroom computers
- Works for computers that may be shared between several teams
- Works for team members’ personally owned computers
- Management and administration is nearly identical for Windows, Macs, and Linux systems
- Setup on multiple computers can be automated through scripts
- Does not require installation of a credential manager (helper application)
- Supports removing access from a personally owned computer when a student leaves the team

Cons:
- Not secure: credentials are stored without encryption on each computer
- Requires some familiarity with Command Line functions

### The Personal Access Token (PAT): Keys to the Kingdom

We use GitHub-generated _Personal Access Tokens_ (PATs) to identify an Android Studio request to GitHub as authentic.
The PAT is a 40 character sequence of mostly random-appearing text, e.g.:

`ghp_PersonalAccessTokenForAnArbitraryxyz`

(The first four characters are always “`ghp_`”.)
Your teams’ GitHub accounts will still have Account Names and Account Passwords, but they will no longer work for Android Studio operations.

### How many PATs do you need?

You will need at least one PAT for each GitHub account.
At The Princeton STEM Academy, we have several shared computers for programming, and like to keep them as interchangeable as possible,
from the FTC programmers’ viewpoint.
With three teams, each computer has the same three PATs for the individual team repositories.
They all get installed the same way, from a script (Windows Command Prompt `.BAT` file).

For individual team members’ personally owned computers, we generate individual PATs, tied to that team member’s identity.
This way, when a team member departs, we can delete only their own PAT, affecting nobody else.
The recordkeeping burden is not large.

In summary, for each team’s GitHub account, there will be individual PATs for each of the programmers, plus one for all of the Academy computers to share.

### Creating a PAT

You can create as many PATs as you need from the GitHub page for your team’s account.
Log in to the account, and open the menu from the profile icon at the top right (1).
Select “Settings” (2).
From the menu on the left, near the bottom, select “Developer settings” (3).

# First two pictures go here
`[Link](url) and ![Image](src)`

On the GitHub Apps page, select “Personal access tokens”.
This next page will list all of the PATs for the account, by the name or comment entered when each was created.
You can delete tokens at will from here - for example, if a programmer has left the team, his / her token should be deleted.
You can also edit access rights by clicking on the name / comment.  For now, click on “Generate new token”.

On the “New personal access token” page, enter a descriptive note for this PAT (4).
For example, a team programmer’s first name, last initial, and team number.
Select an expiration date, or “No expiration” (5).  For “Select scopes”, click “repo” (6), which will populate the entire repository block.
(That’s probably all of the scope a typical FTC programmer will ever need.)

A note on expiration dates: GitHub presents you with a few options, including a custom date, and “No expiration”.
GitHub will warn you about selecting “No expiration”.
We don’t see it as that much of a problem, given the relatively small number of PATs you’ll need, and the ease of tracking.  Your choice.

Scroll down, and click the green “Create token” button when you are done.

### Credential Formats, and Using the PAT

A simple GitHub remote repository credential is just the complete URL.
For team Xenon, with a GitHub account xenon-ftc, their Ultimate Goal repository URL might be:

`https://github.com/xenon-ftc/Ultimate-Goal.git`

With the sample PAT (above), the credential with authentication token might be:

`https://ghp_PersonalAccessTokenForAnArbitraryxyz@github.com/xenon-ftc/Ultimate-Goal.git`

Note the “at” sign (“`@`”) between the PAT, and the rest of the URL.
(For the record: if Account Name / Password authentication were still accepted, the format would have a colon separating the Account Name and the Password:)

`https://AccountName:Password@github.com/xenon-ftc/Ultimate-Goal.git`

### Cloning with a PAT

The easiest way to incorporate the PAT into a local Android Studio project is to do it while cloning the remote repository.
Open a Command Prompt in your project’s parent directory of choice.  Instead of:

`git clone https://github.com/xenon-ftc/Ultimate-Goal.git`

Do this:

`git clone https://ghp_PersonalAccessTokenForAnArbitraryxyz@github.com/xenon-ftc/Ultimate-Goal.git`

(All on one line.)

From inside the newly-created project folder, you can confirm that the PAT is now part of the project with:
`git remote -v` Which will show something like:

```
origin  https://ghp_PersonalAccessTokenForAnArbitraryxyz@github.com/xenon-ftc/Ultimate-Goal.git (fetch)
origin  https://ghp_PersonalAccessTokenForAnArbitraryxyz@github.com/xenon-ftc/Ultimate-Goal.git (push)
```

That’s all there is to it.  You can start Android Studio, open the Ultimate-Goal project,
and Android Studio will be able to Update and Push code to GitHub without further Account Name or Password prompts.
All of the authentication is in the PAT.


### Working with an Existing Project

If you already have an Android Studio project and want to add a PAT to the credentials:
The sure-fire way to make it work is to delete the old project folder, and clone a new project folder, using the procedure above.

**precaution:**  Please make sure that Android Studio does not have the project open when you delete and re-clone.
Use `File | Close Project` in Android studio to avoid conflicts.

You may find a situation where deleting the old content on the computer would result in loss of work,
perhaps because somebody forgot to “Push” their work at the end of last season.
You can update the remote references to save that work, but it requires a little more Git command line skill (example):

```
git remote rename origin origin_old
git remote add origin https://ghp_PersonalAccessTokenForAnArbitraryxyz@github.com/xenon-ftc/Ultimate-Goal.git
git remote remove origin_old
git push
```

The last command will fail because there’s no recorded upstream branch for the new remote repository.
The error message will contain the text of the command you should run to fix that:
copy it out of the message, and paste it back at the prompt to run it.

Or, at your own risk, you could edit the configuration file directly.
From a Command Prompt in your project folder, cd .git (that’s “dot git”, not “git:) to find the git local configuration file, config.
Make a backup copy!  Open it with a plain text editor, like Notepad:

`notepad config`

Contents will be something like this:

```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://github.com/xenon-ftc/Ultimate-Goal.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
        remote = origin
        merge = refs/heads/main
[branch "GlobalClass61"]
        remote = origin
        merge = refs/heads/GlobalClass61
```

Find the url line, and paste in your PAT:

```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://ghp_PersonalAccessTokenForAnArbitraryxyz@github.com/xenon-ftc/Ultimate-Goal.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
        remote = origin
        merge = refs/heads/main
[branch "GlobalClass61"]
        remote = origin
        merge = refs/heads/GlobalClass61
```

This is obviously a bit of a risk: these files are not meant to be edited by a human.
But it should work, and may save you the trouble of fixing the upstream branch records if you have lots of branches.
