Here is a comprehensive, project-based Git tutorial documentation. To make this practical, we will build a simple project called "Recipe Book" and walk through the entire Git lifecycle.

1. Introduction & Setup
Git is a distributed version control system that tracks changes in your source code during software development. It allows you to roll back to previous states, branch off to try new ideas, and collaborate with others without overwriting each other's work.

Initial ConfigurationBefore running commands, tell Git who you are. Open your terminal or command prompt and run:

git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

2. Phase 1: Starting Your Project (The Local Workflow)Let's create our project directory and see how Git tracks files.

Step 1: Initialize the Repository 
Create a new folder for your project, navigate into it, and initialize Git.

mkdir recipe-book
cd recipe-book
git init

What happened? git init creates a hidden .git folder inside your directory. This folder acts as Git's "brain," tracking every modification you make from this point forward.

Step 2: Create a File and Check Status Create a simple HTML file called index.html:

<!-- index.html -->
<!DOCTYPE html>
<html>
<head><title>My Recipe Book</title></head>
<body>
    <h1>Grandma's Secret Recipes</h1>
</body>
</html>

Now, check the status of your working directory:
Bash git status

Explanation: Git will show index.html in red under "Untracked files". This means Git sees the file, but it isn't taking snapshots of it yet.

Step 3: Staging and Committing To save a snapshot of your project, you must first stage the file (put it in the staging area) and then commit it (save it permanently to history).Stage the file:
Bash
git add index.html

(Alternatively, use git add . to stage all new/modified files in the current directory).Commit the file:Bash

git commit -m "Initial commit: Create recipe index page"

The Analogy: Think of git add as packing items into a box for moving, and git commit as sealing the box and writing a label on it.

Step 4: Reviewing Your HistoryTo see the timeline of your project, use:
Bash git log
This will display the unique commit ID (SHA-1 hash), the author, the date, and your commit message.

3. Phase 2: Branching and Feature Development

Branches allow you to work on new features (like adding a dark mode or a new recipe page) without breaking the stable code on your main timeline.

Step 1: Create a New BranchLet's say we want to add a chocolate cake recipe. We will create a branch called feature-cake.

Bash
git switch -c feature-cake
(Note: Older versions of Git use git checkout -b feature-cake).

Step 2: Make Changes on the BranchCreate a new file named cake.html:HTML
<!-- cake.html -->
<h1>Chocolate Cake Recipe</h1>
<p>1. Mix flour, sugar, and cocoa.</p>
Stage and commit this new file:

Bash
git add cake.html
git commit -m "Add chocolate cake recipe page"

Step 3: Merging BranchesNow that the cake recipe is complete, we want to bring it back into our stable timeline (main branch).Switch back to your main branch:Bash

git switch main
(Notice that cake.html temporarily disappears from your folder—this is normal! It only exists on the other branch right now).Merge the changes:Bash

git merge feature-cake

cake.html is now safely combined into your main branch. You can safely delete the feature branch if you are done with it:

Bash
git branch -d feature-cake

4. Phase 3: Handling a Merge Conflict

A Merge Conflict occurs when two branches modify the exact same line of a file, and Git doesn't know which version to keep.Let's trigger a conflict on purpose:On the main branch, change the heading in index.html to <h1>The Ultimate Recipe Book</h1>. Commit this change (git commit -am "Update heading on main").Create and switch to a quick branch: 

git switch -c adjust-title. 

Switch back to main? No, let's pretend we are on adjust-title and change that exact same heading to <h1>Grandma's Best Recipes</h1>. Commit it.

Go back to main: 
git switch main
Try to merge: git merge adjust-title.
Resolving the Conflict Git will stop the merge and output a message saying Automatic merge failed; fix conflicts and then commit the result.
Open index.html. 
You will see conflict markers injected by Git:
HTML<<<<<<< HEAD
<h1>The Ultimate Recipe Book</h1>
=======
<h1>Grandma's Best Recipes</h1>
>>>>>>> adjust-title
<<<<<<< HEAD: 

Points to the changes on your current branch (main).=======: The divider between conflicting changes.>>>>>>> adjust-title: Points to the changes coming from the branch you are trying to merge.To fix it: Edit the file manually. Remove the markers (<<<<<<<, =======, >>>>>>>) and keep the version you want (or combine them).HTML<h1>The Ultimate Recipe Book & Grandma's Favorites</h1>
Save the file, then stage and commit to complete the merge:Bash

git add index.html
git commit -m "Fix merge conflict in index title"

5. Phase 4: Collaboration & Remote Repositories

To share your code on platforms like GitHub, GitLab, or Bitbucket, you link your local repository to a remote server.Push Code to a RemoteCreate a blank repository on GitHub. Copy the repository URL. Link your local repository to GitHub:Bash

git remote add origin https://github.com/your-username/recipe-book.git
Push your code for the first time:Bash
git push -u origin main

The -u flag sets the default upstream branch so that next time you only need to type git push.Fetching and Pulling Team Changes If a teammate updates the remote repository, you bring those changes down to your computer using:Bash

git pull origin main
Cloning a Project If you want to download an existing project from GitHub to work on it:Bash
git clone https://github.com/username/project-name.git

Git Command Cheat Sheet Command What it does 
git init Starts a new local Git repository.
git status Shows what files Git is tracking and what has changed.
git add <file> Moves changes from the working area to the staging area.
git commit -m "msg" Permanently saves staged changes to the history timeline.
git log Displays a list of all past commits in chronological order.
git switch -c <name> Creates a new branch and instantly switches your workspace to it.
git merge <branch> Merges the specified branch's changes into your current branch.
git push Uploads your local commits to a remote server (e.g., GitHub).
git pull Downloads and merges the latest updates from the remote server.


Advanced Git Tutorial: 
"Recipe Book" Project

Now that you have mastered the foundational Git workflow (adding, committing, branching, and basic merging), let’s elevate your skills. In production environments, projects get messy. You will need to rewrite history, clean up commits, save unfinished work temporarily, and surgically move changes around.We will continue using our "Recipe Book" project to explore these advanced Git techniques.

1. Interactive Staging (git add -p)

Sometimes, you make multiple unrelated changes inside a single file, but you don't want to commit them all at once. Interactive staging allows you to review your changes chunk by chunk (called hunks) and choose exactly what goes into the next commit.The ScenarioYou open index.html and make two distinct updates:You add a CSS link in the <head>.You add a new footer at the bottom of the <body>.Instead of committing both together, you want to split them into two clean, logical commits.

The Command Bash
git add -p index.html

How it WorksGit will display the first modification hunk and prompt you with a question: Stage this hunk [y, n, q, a, d, j, J, g, /, e, ?]?Key options include:y: Yes, stage this hunk.n: No, do not stage this hunk.s: Split the current hunk into smaller pieces (useful if changes are close together).q: Quit interactive mode (saves whatever you've already staged).Result: You can press y for the CSS link change, and n for the footer change. Running git status will show index.html simultaneously under "Changes to be committed" and "Changes not staged for commit". 

You can now run git commit -m "Add CSS styling link" and then make a second commit for the footer.

2. Managing Temporary Work with Stashing

Imagine you are in the middle of refactoring your cake.html file. It's totally broken and half-written. Suddenly, a critical bug is found on the main branch that requires an immediate fix. You cannot switch branches with broken, uncommitted work without risking a mess.The Solution: Stash your messy code away in a temporary clipboard, fix the bug, and then bring your messy code back.

Step 1: Save your progress to the stash
Bash
git stash -m "WIP: redesigning cake recipe layout"

Your working directory is now completely clean. Your half-written code is safely tucked away.

Step 2: List your stashes If you do this multiple times, you can view your clipboard history:

Bash
git stash list

It will display something like: stash@{0}: On main: WIP: redesigning cake recipe layout

Step 3: Bring your work backOnce you finish fixing the bug on main, switch back to your development branch and run:Bashgit stash pop

Note: pop restores the changes and deletes them from the stash list. If you want to restore the changes but keep them in the stash file, use git stash apply instead.

3. Rebasing vs. Merging 

While git merge combines branches by creating a special "merge commit" (joining two timelines together), git rebase rewrites history by moving your entire feature branch so it begins on top of the latest commit on main.The Visual Difference Using Merge: Your history branches out and converges, leaving a commit trail showing exactly when branches merged.

Plaintext      
A---B (feature)
     /     \
D---E-------F (main)  <- F is the merge commit

Using Rebase: Your history is rewritten into a perfectly straight line, making it look as if you built your feature on top of the newest code from day one.
Plaintext            
A'---B' (feature shifted to the tip)
           /
D---E-----F (main)
The Workflow To rebase your feature-cake branch onto main:
Bash
git switch feature-cake
git rebase main

The Golden Rule of Rebasing: Never rebase branches that have already been pushed to a public remote repository (like GitHub) and are being used by other developers. You will rewrite history that they rely on, causing massive synchronization issues.

4. Interactive Rebasing (git rebase -i)

Interactive rebasing is the ultimate history-cleaning tool. It allows you to look back at your local commit history and rewrite it before pushing it to GitHub. You can combine small commits, change commit messages, or delete accidental commits.The ScenarioWhile working on a new soup recipe, your git log --oneline looks like this:Plaintexta1b2c3d Add soup recipe file
e5f6g7h Fix typo in soup ingredients
j9k0l1m Oops forgot salt, adding it now
You don't want your team to see your typos and forgot-salt mistakes. You want to combine these three messy commits into one clean commit.The CommandTo open the interactive menu for the last 3 commits:
Bash
git rebase -i HEAD~3

The InterfaceA text editor will open displaying your commits upside down (oldest at the top):
Plaintext
pick a1b2c3d Add soup recipe file
pick e5f6g7h Fix typo in soup ingredients
pick j9k0l1m Oops forgot salt, adding it now

To clean this up, change the word pick to squash (or just s) for the minor adjustment commits. Squashing combines that commit into the one directly above it.Plaintextpick a1b2c3d Add soup recipe file

squash e5f6g7h Fix typo in soup ingredients
squash j9k0l1m Oops forgot salt, adding it now

Save and close the file. Git will then prompt you to write a new, unified commit message. You can write: Add completed soup recipe with all ingredients. Your history is now perfectly clean!

5. Cherry-Picking (git cherry-pick)Sometimes you don't want to merge or rebase an entire branch. Instead, you just want to grab one single commit from someone else's branch and paste it onto your current branch.The ScenarioA teammate is working on a massive experimental branch called experimental-kitchen. Inside that branch, they wrote a brilliant commit that updates the css-styles file to fix a broken navigation bar. You need that navbar fix right now on main, but you don't want their experimental cooking features.The WorkflowFind the commit hash of the specific change (e.g., 9a8b7c6).Switch to your target branch:
Bash
git switch main

Pull just that commit into your timeline:
Bash
git cherry-pick 9a8b7c6

Git will copy that exact change and generate a brand-new commit on main.

6. Undoing Mistakes (Reset vs. Revert)

When things go wrong, Git provides two distinct ways to undo changes, depending on whether the code is local or already shared publicly.

Option A: git reset (For Local Changes Only)If you haven't pushed your code to GitHub yet, you can completely undo commits.Soft Reset: Undoes the commit, but keeps your code changes staged in your editor.

Bash git reset --soft HEAD~1
Hard Reset: Completely obliterates the last commit and deletes all code changes associated with it. Use with caution.
Bash
git reset --hard HEAD~1

Option B: git revert (For Public/Pushed Changes)

If you already pushed a bad commit to GitHub, using git reset will break your teammates' history. Instead, use git revert.Bashgit revert 4c3b2a1

What it does: Instead of deleting the bad commit 4c3b2a1, Git calculates the exact inverse of those changes and creates a brand-new commit that rolls them back. This preserves historical integrity without deleting old timeline nodes.

7. The Ultimate Safety Net: git reflogHave you ever run a git reset --hard by accident and panicked because you thought you deleted days of unpushed work?Take a deep breath and run:

Bash git reflog

Explanation While git log shows your project's commit history, git reflog tracks every single action your local Git head took, including switching branches, rebasing, committing, and resetting.Even if a commit is completely unlinked from your branch history due to a hard reset, it still lives in Git's background database for a few weeks. You can find the hash of the commit you "deleted" inside the reflog list and rescue it:

Bash git switch -c rescued-branch <commit-hash-from-reflog>

Advanced Operations ReferenceAdvanced CommandUse Case Scenario
git add -p
When you want to break down multiple changes in a single file into separate commits.

git stash / git stash pop
When you need to clear your workspace to fix an emergency bug without committing broken work.

git rebase main
When you want to update your feature branch to line up linearly with the latest master commits.

git rebase -i HEAD~X 
When you want to squash, rename, or reorder your local commits before making a pull request.

git cherry-pick <hash>
When you need to steal a single bug-fix commit from an unfinished experimental branch.

git revert <hash>
When you need to undo a bad change that has already been pushed to production/GitHub.

git reflog
The nuclear emergency button used to recover commits or branches lost via accidental hard resets.

