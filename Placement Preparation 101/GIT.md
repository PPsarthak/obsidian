#placement-preparation  #git 
##### Connecting local repo to remote repo
Check remote config - `git remote -v`
Add remote URL - `git remote add origin <url>

---
Pull changes from remote to local - `git pull --rebase`
Rebase your local changes on it - `git rebase origin/main`
Resolve the merge conflicts manually. Once done, commit if required
Continue rebasing - `git rebase --continue`
Push changes -  `git push --set-upstream origin <branchName>` / `git push -u origin <branchName>`

---
#### Setting up Git config
```bash
git config --add --system user.name "github_username_here"
#sets system level config properties : for all users

git config --add --global user.name "github_username_here"
#sets config properties for curr user

git config --add --local user.name "github_username_here"
#sets config properties for curr repo. If no flag is provided, git assumes local
```

```bash
git config --get user.name
```

```bash
git config --global init.defaultBranch master
```

> [!note] Main vs Master
>  <span style="color:rgb(255, 192, 158)">Git's default branch is master, Github's default branch is main</span>
#### Initializing Git Repository (Local)
```bash
mkdir <folder-name>
git init
git status
```
### Git basics

There are 3 stages in Git: <mark style="background: #33EE99;"><span style="color:rgb(0, 0, 0)">Modified → Staged → Committed</span></mark>

-  Modified
	-  When you make any change — create a new file, modify an existing one, or delete a file — it is stored in the Modified stage.
	-  By default, Git tracks all files inside your repository. So, whether you create a `.txt, .env`, or any other file, Git will notice any change — creation, update, or deletion
	- Any file which is tracked by Git is c/a <span style="color:rgb(240, 81, 105)">Tracked File</span> and any file which is not tracked by Git is c/a <span style="color:rgb(240, 81, 105)">Untracked File</span>
	
> [!info] Marking files as untracked 
>  To prevent Git from tracking specific files or file types, you can add them to a `.gitignore` file. This file tells Git to ignore those paths and not include them in version control.
>  When you run the `git init` command to initialize the local repository, it creates a `.git` hidden folder; the `.gitignore` is not created then (I thought it is created 😅). 
>  You need to manually create it and append the file that you want to untrack -
>  `touch .gitignore` 
>  `echo ".env" >> .gitignore` 
>  `echo "*.log" >> .gitignore`
>  
>  **But remember, it works iff the file is never tracked by git earlier i.e., you need to already specify to ignore these files from version control before even creating/adding in the first place.**
>  If a file was already in version control first, and then you add it to `.gitignore`, it won’t stop Git from watching it — it’ll still show as modified
>  
>  `git update-index --skip-worktree` works on tracked files already in the repo. Tells Git: “Keep tracking this file in history, but ignore local modifications.”

 -  Staged 
	 -  When you run `git add <file>`, the modified file moves to the Staged (or Index) stage. 
	 - Basically, it marks the file to be included in the next commit. (Remember, when committing the files, you only specify the message not the files to include for commit; because it is here where you specify the files to commit)
-  Committed
	-  When you are ready to take a snapshot of the changes you have made, you can run `git commit -m "message"` to make a commit (take a snapshot of the state of all the staged files)

> [!imp] Scenario
> You created a `.txt` file and added a simple message to it and commit it.
> Then you modify the file and run `git status`. It shows you 2 suggestions:
> 	1.  `git add <file>`: You know what it do
> 	2. `git restore <file>`: It will restore the file to previous commit state (undoing all the changes)
> 
> Now if you run `git add <file>` and stage the changes and then run `git status` It shows you 1 suggestion:
> 	1. `git restore --stage <file>`: It will remove the file from staging

There are 4 types of files you can see in git : 
- **Untracked Files**: They aren’t part of the version control.
- **Tracked Files**: Git is tracking changes of these files - create/update/delete
- **Staged Files**: These are files added to the staging area using `git add`. They are marked for inclusion in the next commit. 
- **Committed Files**: These are files that have been saved to the Git repository's history. They are safely stored and versioned.

> [!Important] Amend Commit
> `git commit --amend`: Replace the last commit with = staged changes + last commit combined. <span style="color:rgb(106, 240, 214)">If nothing is staged, use it to edit last commit message</span>
## Git Logging

`git log --oneline` one line logs
`git log branchname` logs for that branch only
`git log -n 10` shows the last 10 commits !!!!
## git reset command
There are 3 flags for the `git reset <commitId>`:
- `--soft`
- `--mixed`: also the default i.e., if you use only `git reset`, mixed flag will be applied
- `--hard`

**Understanding in 2 different scenario: You have 2 commits: A and B and you are running**
*Scenario 1. Commit A: Created a `names.txt` file Commit B: Deleted the `names.txt` file*
`--soft` :- the deletion is still in the index/project → `git status` shows the file deletion as staged (ready to re-commit)
`--mixed`:- shows the file as deleted (not staged); you can `git restore <file>` to bring it back, or `git add` + `git commit` to record the deletion
`--hard`:- the file is restored on disk exactly as in A. Any local changes are discarded

*Scenario 2. Commit A: Created a `names.txt` file Commit B: Created a `surnames.txt` file*
`--soft` :- file2 is present and staged (ready to re-commit)
`--mixed` :- file2 is not staged but exists in working tree (you need to `git add` + `git commit`)
`--hard`:- file 2 is removed from working tree/folder (you will need to create it again)
##### My Summary Notes:
`--soft`: undoes `git commit` command and hence, the deleted file is available on stage (ready to re-commit)
`--mixed`: undoes `git commit` and `git add` command and hence, the deleted file is not on stage (so you can git add + git commit)
`--hard`: undoes `git commit`, `git add` & `delete/create (actual file operations)` operation and hence, the project is same as when it was commit A

> [!important] Downside of using `git reset <commitId>`
> It undoes/deletes any commits after the given commitId
## git revert command
`git revert <commitId>` creates a new commit that reverses the changes made by a previous commit.
It doesn’t delete or modify existing commits — instead, it adds a new commit on top of the current branch that undoes what the chosen commit did.
So if:
	Commit A → added a file
	Commit B → deleted that file
	Then running git revert B will create Commit C that adds the file back (opposite of B).
## git update-index --skip-worktree
`git update-index --skip-worktree <file>`:  This command tells Git:
> “Pretend this file never changes — don’t notice or show modifications to it in git status, and don’t include it in commits.”
#### 🔍 How it works
- The file remains in the repo and is versioned (it will still exist as tracked for others who clone or pull from remote).
- But your local Git will stop checking for modifications to it.
- Any changes you make to that file stay local — git status won’t show them, and git commit won’t include them.

> [!scenario]
So, if Person A clones the repo from remote and runs `git update-index --skip-worktree app.properties` , even if make changes to `app.properties`, git won't track them and won't commit them
> But if Person B clones the repo from remove and makes changes to `app.properties`, he can accidently stage/commit/push them to remote

**To resume tracking, run** - `git update-index --no-skip-worktree <file>`

🧩 Key Takeaways
- `--skip-worktree` is a personal safeguard, not a shared configuration — it only affects your local Git index.
- Each developer who wants to “protect” their local version must run it themselves
- You should run this command for `application.properties, .env` files 

## Git Stash
`git stash` in Git and “shelve changes” in IntelliJ are conceptually the same thing — both let you temporarily save uncommitted work so you can switch contexts (like changing branches) without losing your changes.

> [!abstract] 🧩 Scenario
> You’re working on a Spring Boot project on the feature/login branch and you’ve modified a few classes
> But suddenly, you need to switch to the main branch to fix a critical bug.
> However, your current changes are uncommitted — if you try switching branches, Git warns you because the working directory isn’t clean 
> 
> You can run `git stash` to save/stashe your uncommitted changes into a temporary stack and restores your working directory to a clean state (like the last commit)
> Later, you can run `git stash pop` to re-apply the changes and remove the stash from that temporary stack
> By default, git stash only stashes:
> - Tracked files that are modified even if unstaged - (files that were committed earlier and now are modified; but didn't ran git add)
> - Staged changes (from git add)
> It does stashes:
> - Untracked files (like newly created)
> - Ignored files (listed in .gitignore)
> 
> > [!todo] Even more pro tip...
> > If you want to keep the stash for later (don’t remove it), you can run - `git stash apply` to re-apply the changes but keep the stash in that temporary stack

Common Commands:
```git
git stash -u             # Stash untracked files as well
git stash list           # Show all stashed changes
git stash show -p        # View what’s inside the stash
git stash drop <stash@{n}>  # Delete a specific stash
git stash clear          # Remove all stashes
```

## Git Remote Connection
#### ⚙️ Steps to Connect Local Repo → Remote Repo
1. Create a remote repository and copy the URL or SSH
2. On your local, run - `git remote add origin <url>`
3. Verify the connection, run - `git remote -v`
4. Push the local commits to remote, run - `git push -u origin main`

> [!note] 
>  In the command above, the `-u` flag in `git push` sets origin/main as the default upstream branch for future pushes

## Git Branching

- `git branch -m oldname newname` rename a branch
- `git branch` shows the current working branch
- `git branch -r` see all remote branches
- `git branch -l` / `git branch`  see all local branches
- `git branch --all` see all branches
- `git branch -d branchName` deletes the branch
- `git checkout branchName` checks out to the given branch
- `git checkout -b branchName` creates a branch with given name

## git fetch vs git pull
`git fetch`: Downloads latest changes (branches, commits, tags) from remote
`git pull`: Does everything git fetch does + automatically combines the fetched changes into your current branch (could create conflicts)

> [!scenario] Scenario
> Your last local commit: C1, Remote has moved ahead with two commits: C2, C3
> If you run `git fetch origin`, it downloads the new commits C2, C3 from remote; but your local branch is still at C1.
> You can run - `git log <branchName>..origin/<branchName>` to see what changed and later merge with `git merge origin/main`
> 
> If you run `git pull origin`, it downloads the new commits C2, C3 from remote + merges (could create conflicts) so that you local branch can point to C3

## Merge Conflicts
A merge conflict happens when Git can’t automatically combine changes from two branches because the same part of a file was edited differently in both branches. Git is smart enough to merge most things automatically, but if both branches modify the same lines or delete/rename the same file differently, it needs you to decide what to keep.
#### Git marking
When conflicts arise, git marks the file. Example:
```java
public String greetUser() {
<<<<<<< HEAD
    return "Hello, User!";
=======
    return "Welcome User!";
>>>>>>> main
}
```
- The section between <<<<<<< HEAD and ======= is your version.
- The section between ======= and >>>>>>> main is the other branch’s version.

> 💡 Tip: Always use your IDE (like IntelliJ) — they show conflicts visually, making it much easier to pick the right lines.

## Merge

`git merge <branch>` merges the specified branch onto the current branch

> [!example]+ Merge Conflict
> ![[Pasted image 20241123165458.png]]
> This command will find the best common ancestor of both branches, which is A here. From there it will replay all the changes and try to record all of the changes into a new commit F.
> If any problem arrives, it is called *merge conflict*
> Merge conflict : when 2 different branches (which are getting merged) have committed 1 single line

### Fast Forward Merge Technique

![[Pasted image 20241123170519.png]]
This happens when there is no progress on main branch, so a simply updating the HEAD ptr will merge.
Imagine you create a branch from main, within the next min, you add a print statement and hit merge!! There is nothing new added on the main (from pt B). So you created branch from B and there is no progress on main from B

There is no merge commit here
### Git Rebase

Git merge and git rebase go hand-in-hand. They can both can be used to merge your branch into another one, Say we have the following structure

```
A - B - C    main
   \
    D - E    feature_branch
```

Now if you use git merge, a new commit 'F' will be created that merges the 2 branches.
If you use git rebase, this will happen: 

```
A - B - C         main
         \
          D - E   feature_branch
```

So as you can see, git rebase allows you to use a fast forward merge.
Also there is no merge commit (having 2 parents)
The major advantage here is that, it is easy to use `git revert <commitHash>` to undo the changes caused by feature branch, since it is fast forward merge


> [!tip] Conflicts
> If you get a merge conflict while rebasing, you will for sure get a conflict while using git merge
> Think about it, 
> ```
> A - B - C         main
 >        \
 >         D - E   feature_branch
>```
>If you had a merge conflict while doing this, it is possible that 1 file is changed in commit B/C and the same file is changed in commit D/E.
>So now think, even if you did git merge, you would get the merge conflict

### Git hashes

A hash is created whenever you commit. Each being unique and uses SHA-1
2 people commiting the same thing will still end up with different hash because the hash takes in picture:
- The author name and mail
- Commit message
- Date and time
- Previous commit hashes

> Use `git cat-file <hash>` to see the contents of the commit hash. 
> You will get a tree object. Go inside the tree with same cmd (replace the hash)
> You might see blob that stores the actual file

### Workflows - Encountering Merge Conflicts

> [!scenario] Scenario
> You are working on a feature; so you create a feature branch on remote and then get that branch on local
> You work on local branch and haven't pushed any changes to remote branch
> Remote branch's only responsibility is to be in sync with the remote main branch
> 
> Now your development is finished on local branch and you are ready to push to remote branch

1. Before pushing local changes to remote branch, make sure it is synced with remote main branch
2. Now pull the changes from remote to local:
	1. Merge Strategy: Using `git pull`:
		-  IntelliJ will automatically show merge conflicts
		-  Resolve them in IntelliJ 
		-  Continue the merge
		-  You will need to make 1 more commit to finish (because; here the merge strategy is used by default in git pull)
	2. Rebase Strategy: Using `git fetch`:
		-  Fetch the changes on local 
		-  Then run - `git rebase origin/feature` - this replays your local commits on top of the remote branch’s current tip (which already contains the synced main)
		-  You get conflicsts and you resolve them in IntelliJ
		-  Now run - `git rebase --continue`
3. In either case now, push the changes to remote; `git push origin feature`

> [!note]
> - If you used rebase but had previously pushed this branch to remote (and you rewrote history); then run - `git push --force-with-lease origin feature`
> - `--force-with-lease` updates remote only if it still points to the commit you expect; it is safer than `--force` because it avoids clobbering others’ concurrent pushes
> - If you use merge; then `git push origin feature` does the work