# Git & GitHub Training Report

**Student Name:** \[elsayed alaa elsayed\]  
**Repository URL:** \[Repo Link Here\]

---

## 📌 Part 0: Conceptual Understanding

### 1\. Git vs. SVN & Storage Models

* **Centralized (SVN) vs. Distributed (Git):**  
    
  > \[SVN using one server to hold all data and you need connection to edit in the repo . git lets you have you own copy of the repo that you can edit,commit and branch\]  
    
* **Deltas vs. Snapshots:**  
    
  > \[deltas is the defrense between the first file and the updated file . snapshots saves the full file with every update  \]

---

## 🛠️ Task 1: "Git Lab" (Internals, Local Workflow, Undoing, Tags)

### 1\. Git Identity & Initialization

* Output of `git config --list --show-origin`:

\# file:C:/Users/HP/.gitconfig     user.email=elsayedswalm@gmail.com
\# file:C:/Users/HP/.gitconfig     user.name=elsayed alaa
### 2\. Git Architecture & Internals

* **Object Hashes:**  
    
  * Blob SHA: `[ bd652635afa06c8862afb6ae96667f70dc85f95a]`  
  * Tree SHA: `[cc0211b56bde3eb7e74219b9e2647cbaee6c69eb]`  
  * Commit SHA: `[8a39fb804e3b86771292c905c7eb48e95a4bae90]`


* **Object Content Inspection (`git cat-file -p`):**

\# tree cc0211b56bde3eb7e74219b9e2647cbaee6c69eb
\#author elsayed swalm <elsayedswalm@gmail.com> 1791139697 +0300
\#committer elsayed swalm <elsayedswalm@gmail.com> 1791139697 +0300
\#first

\#100644 blob bd652635afa06c8862afb6ae96667f70dc85f95a    test.txt

\#hi my name is elsayed

* **Object Type Inspection (`git cat-file -t`):**
\#git cat-file -t 8a39fb804e3b86771292c905c7eb48e95a4bae90
\# commit
\#git cat-file -t cc0211b56bde3eb7e74219b9e2647cbaee6c69eb
\#tree

\# git cat-file -t bd652635afa06c8862afb6ae96667f70dc85f95a
\#blob

* **Staging Area Verification (`git ls-files -s`):**

\# 100644 38744c1ffcff40b58a18e45a108fc68390348cd6 0       test.txt

* **Brief explanation of the relationship between them:**  
    
  > \[commit is the wrapper of the tree and blobs and it contans the history, tree in the directory that contans blob and other trees , blob is the snapshot of the file that has the content ]

### 3\. Ignoring Files (`.gitignore`)

* Output of `git status` proving `.env` and temporary files are ignored:

\#PS D:\gitwork> ls


    Directory: D:\gitwork


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         10/4/2026  10:54 PM              6 .gitignore
-a----         10/4/2026  10:55 PM              0 app.py
-a----         10/4/2026   9:50 PM             44 test.txt


PS D:\gitwork> git status -s
?? .gitignore 

### 4\. Inspecting Changes

* Screenshot or terminal output of `git diff` & `git diff --staged`: *(diff --git a/test.txt b/test.txt
index 38744c1..5324b08 100644
--- a/test.txt
+++ b/test.txt
@@ -1,3 +1,4 @@
 hi my name is elsayed
 new line
-new line 2
\ No newline at end of file
+new line 2
+new line 3
\ No newline at end of file)*

### 5\. Undoing Changes & History Recovery

* Demonstrating `git commit --amend`:

\# new line 3

Please enter the commit message for your changes. Lines starting
with '#' will be ignored, and an empty message aborts the commit.

 Date:      Sun Oct 4 23:11:13 2026 +0300

 On branch master
 Changes to be committed:
       modified:   test.txt

* Demonstrating `git reset --hard` and recovery via `git reflog`:

\# 8976c54 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
28dd88c HEAD@{1}: commit: new line 3
8976c54 (HEAD -> master) HEAD@{2}: commit: add gitignore
6dfdb1e HEAD@{3}: commit: last
f55abb8 HEAD@{4}: commit: second
8a39fb8 HEAD@{5}: commit (initial): first

### 6\. Tagging

* Output of `git show v1.0` (Annotated tag):

\# version 1.0 of file

commit 7429a883f41f83dbad749e275ed49e1ed6c8ac08 (HEAD -> master, tag: v1.0)
Author: elsayed alaa <elsayedswalm@gmail.com>
Date:   Sun Oct 4 23:22:48 2026 +0300

    graph added

diff --git a/graph.png b/graph.png
new file mode 100644
index 0000000..70a77aa
Binary files /dev/null and b/graph.png differ

---

## 🌿 Task 2: "Branch Battle" (Branching, Merging & Rebase)

### 1\. Fast-Forward Merge

* Branch merged: `feature/login` into `main`  
* Terminal output / confirmation message:

\# git branch --merged
* master
  testing
### 2\. Three-Way Merge

* Merge commit created when combining diverging branches (`feature/ui` & `feature/api`):

\#  git merge ui api -m "3 way merge"
Fast-forwarding to: ui
Trying simple merge with api
Merge made by the 'octopus' strategy.
 new    | Bin 0 -> 22 bytes
 newapi | Bin 0 -> 12 bytes
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new
 create mode 100644 newapi
PS D:\gitwork> git branch --merged
  api
* master
  ui

### 3\. Merge Conflict Resolution

* Screenshot of the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`): *(![My screenshot](screenshot.png))*  
* Resolved commit message & terminal verification:

\# [master ba24f9e] Merge branch 'test'

### 4\. Linear History (Rebase vs. Merge)

* **Rebase Comparison:**  
    
  > \[rebase creates a linear history by replaying coomits on the lastes base branch, the 3 wayt merge keep track of the real history of branching and creat a new merge commit \]

### 5\. Repository Visual Graph

* Output of `git log --oneline --graph --all`:

\# * e46f412 (HEAD -> master, linear) line 4
* f3adf76 line 3
*   ba24f9e Merge branch 'test'
|\
| * b180774 add 4
* | 4b01614 del 3
|/
*   ca72c25 3 way merge
|\
| * 004e9da api file added
* | e021198 ui new file
|/
* 6d2ce96 login added in testign branch

---

## 🌐 Task 3: "Remote & Open-Source Workflow" (Solo Simulation)

### 1\. SSH Authentication

* Verification output of `ssh -T git@github.com`:

\# Hi elsayedsw33! You've successfully authenticated, but GitHub does not provide shell access.

### 2\. Synchronization: `git fetch` vs `git pull`

* Output showing the difference after a remote commit was fetched:

\#  git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
PS D:\test_remote> git log origin
commit ba2bb0d7f0aa2dc8609be6cd7b3aceda591fff22 (origin/main, origin/HEAD)
Author: elsayedsw33 <elsayedswalm@gmail.com>
Date:   Mon Oct 5 03:14:34 2026 +0300

    Add content to file.txt

commit 3f0f07817b215cde4c9a957be479a0d30c292b26
Author: elsayedsw33 <elsayedswalm@gmail.com>
Date:   Mon Oct 5 03:05:58 2026 +0300

    Initial commit

\#  git pull origin main
From https://github.com/elsayedsw33/remote_repo
 * branch            main       -> FETCH_HEAD
PS D:\test_remote> git log
commit ba2bb0d7f0aa2dc8609be6cd7b3aceda591fff22 (HEAD -> main, origin/main, origin/HEAD)
Author: elsayedsw33 <elsayedswalm@gmail.com>
Date:   Mon Oct 5 03:14:34 2026 +0300

    Add content to file.txt

commit 3f0f07817b215cde4c9a957be479a0d30c292b26
Author: elsayedsw33 <elsayedswalm@gmail.com>
Date:   Mon Oct 5 03:05:58 2026 +0300

    Initial commit

### 3\. VS Code & GitLens Inspection

* Screenshot showing GitLens commit graph and inline blame: *(![My screenshot](screenshot2.png))*

### 4\. Independent Contribution & PR Simulation

* Secondary Repository URL (`dummy-project`): `[https://github.com/elsayedsw33/dummy-project]`  
* Remote Configuration (`git remote -v`):

\# dum_origin      https://github.com/elsayedsw33/dummy-project.git (fetch)
dum_origin      https://github.com/elsayedsw33/dummy-project.git (push)
origin  https://github.com/elsayedsw33/git_lab.git (fetch)
origin  https://github.com/elsayedsw33/git_lab.git (push)

* **Pull Request Link:** `[https://github.com/elsayedsw33/dummy-project/pull/1#issue-5710035013]`  
* Screenshot of the merged PR on GitHub: *(![alt text](<Screenshot 2026-10-05 130223.png>))*