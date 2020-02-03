# A Crude Intro to Git
_Compiled by Rigel Madraswalla, released to the world_

## Intro
Git is a super useful tool, essential to any software/electrical engineering position.
The full book on Git (written by the makers themselves) can be found here: https://git-scm.com/book/en/v2. Below is a super rudimentary rundown on the everyday Git tools you may find useful.

## Clarifications & Philosophies
1. Git is _NOT_ specific to a certain website. Git is a command line tool; Github, Bitbucket, Gitlab are all examples of websites that _HOST_ Git repositories.
2. Git is meant to streamline the development of code between multiple developers. It is designed to maintain version controlled code, and handle merging code that multiple people have edited.
  - However, Git only tracks changes to **tracked files**. You must manually `add` files that you want to be tracked.
3. There are two main pieces to the Git puzzle:
  - The **remote** (or _origin_)- The (typically) online site where the Git server is hosted (Github, Bitbucket, Gitlab
  - The **local** - This is the copy of the repo that every developer has on their own machine. Changes made on the local branch must be `push`ed to the **remote** in order for other developers to see these changes.
4. Development in Git relies upon **branching**; think of a tree with a bunch of different code branches coming out. Each branch can be `checkout`, edited, and then `merge`d back into the the **Master branch**.
  - Typically every new feature being implemented gets its own _branch_, and when the feature is completed & tested, it can be merged back into the **master branch**.
  - During the process of editing, developers may make `commit`s after hitting certain milestones. Ie. "My code finally compiles! Let's commit." `Commit`s are "checkpoints" in your development process. If things go awry, you can always go back to your last commit.
  - After desired development is achieved, a **Pull Request** (or PR, or merge request) can be made. Depending on your access level in your Git repo, you may either be able to approve this merge request yourself (though it is best practice to have others review your code first), or you may have to wait for an admin to approve the request for you.

## Some useful commands
#### 1) Getting started
```
git clone <repo_link>
```
Create a local clone of a remote repository. Typically, if you find a package on Github/Gitlab, you'll see a button that says `Clone or Download`. There, they will list the link to replace `<repo_link>` with. You can either clone using:
- `https` - Which requires that you authenticate with your account information for that server (username/password)
- `ssh` - Which requires that you register **your machine** with your account on that server using and [SSH Key](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

Use `git clone` if the remote repo **already exists**.

```
git init
```
Initialize the folder that you're currently navigated to in your filesystem as a git repo. This will create a `.git` folder (which will probably be hidden) inside your current directory housing all the info that git needs. Note that in order to `push` to a remote, you will first have to define a remote repo to push to.

#### 2) Getting your bearings
```
git status
```
Run this command to see an overview of all of the changes that you've made so far. It will show you all tracked files that have been edited, and list all untracked files present in the repo.

```
git branch [-r]
```
Lists all the branches present in your _local_ Git repo. Add `-r` to view all _remote_ (online) branches. Also will tell you what branch you currently have checked out.

```
git diff [filename]
```
Gives a line-by-line rundown of all the changes made to _tracked_ files since the last commit.

```
git log
```
Shows the commit log of your repo, allowing you to quickly identify on which commit the _HEAD_ of each branch is located.

#### 3) Making local changes
```
git add [-u | <filename>]
```
- For **tracked** files - Adds any changes made since last commit, "staging" them for the next commit. Use the `-u` option to add _all_ changes made to tracked files since last commit.
- For **untracked** files - Adds `filename` to the list of tracked files, staging it for the next commit.

```
git commit [-m]
```
Create a new commit with all files that have been added. Commit will be added to the current branch. Every commit _requires_ a commit message.
- By default, running `git commit` will bring you to your default command line text editor (VIm, nano, emacs) and you will add your message there.
- Add the `-m` option and add a `'message'` (note the single ' ') for a single line message (generally frowned upon in the professional world) (But super quick and easy).

```
git merge <branch_name>
```
Pull all changes from `<branch_name>` into your current branch. Any files that have been changed in _both_ places (called **merge conflicts**) will have to be manually resolved.  Great if another developer makes a change in their feature branch that you want to test out in your branch.

```
git stash
```
Take all changes since last commit, and push them onto a stack. This reverts all changes since last commit, but keeps them in a place where they can be returned to. Less permanent than a commit; often used when you have to `checkout` a different branch really quick, as you are not allowed to checkout if doing so will overwrite uncommitted changes.

```
git stash apply
```
Pop the local changes that you previously `stash`ed and restore them, so you may pick up where you left off.

```
git rebase [-i | <branch_name>]
```
Use this tool (preferably in interactive mode, `-i`) to restructure the commit history of a branch. Commits here can be squashed (combined together for neatness), erased, re-messaged, among other things. Great if you want to tidy up your branch before merging.

#### 4) Branching

```
git checkout <branch_name>
```
Switch to a different `<branch_name>`. NOTE that you may not checkout a different branch if doing so will overwrite any uncommitted changes (see `git stash` above).

```
git checkout -b <branch_name>
```
Make a _new_ branch of name `<branch_name>`. This will create a branch off of the current branch, bringing in any uncommitted changes with it.
- Note that when running `git push` on a new branch, there will not yet be a corresponding branch on **origin**. Thus, the first time you push, you will be prompted to instead type `git push --set-upstream origin <branch_name>`

```
git checkout -- <file_or_folder>
```
Erase all changes to `file_or_folder` since the last commit. _SUPER_ useful tool for saying "forget it, I'm starting over from working code". **CAREFUL**: There is **no way** to recover changes from this command, so be careful with wildcards (`*`, `.`, etc.)

```
git checkout <branch_name> -- <file_or_folder>
```
The more generic variant of the previous: pull `file_or_folder` from the `branch_name` branch into your current branch. The resulting file will automatically be staged for commit in your current branch. Again **CAREFUL**, this will overwrite any uncommited changes!

#### 5) Syncing local repo with remote repo

```
git fetch [fork_name branch_name]
```
Connects to remote repo, and merely informs local Git repo of any changes that have been pushed to remote. It _does not_ actually merge any of these changes in as yet, however. Fetch often, there's no harm in it.

```
git pull [fork_name branch_name]
```
Actually _integrates_ any changes that exist to your branch from remote. If there are any conflicts, you will have to manually resolve them, telling Git which change to accept in every conflicting file. Often, you will be required to perform a `git pull` before doing a `git push` so that if there are any conflicts, you resolve them yourself before `push`ing to origin.

```
git push [fork_name branch_name]
```
Push any new commits you've made on your local repo up to your remote repo for all to see. Basically, if your hard drive were to corrupt itself here, you'd be able to pull all of your code onto another machine no problem! If there have been changes to the remote repo since your last `git pull`, you will be required to `pull` first before `push`ing. This allows you to clear up any potential conflicts (files that have changed both remotely and locally) before pushing your code up to the remote repo.

```
git remote [-v]
```
View all of the _forks_ that Git is currently tracking. [What's a fork?](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/working-with-forks)

There are two important _fork_ names to know:
1. `origin` - The online **copy** of the repo you currently have cloned. You likely have rights to push and pull to here to your whim. `git push`, `git pull`, and `git fetch` all default to the `origin` fork.

2. `upstream` - The repo of code that your local code _comes from_. For example, you clone a package from Github written by SWRI. You, being the new developer you are, cannot directly push to this repo, you'd break SWRI's codebase. You can, however, list their Github repo as the `upstream` fork, and whenever they push new changes, you can `git pull upstream master` to pull their code into your repo.
