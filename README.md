# git_test
Repository for testing Git commands and typical workflows as part of the Master in Robotics by UVic

# Testing Git: create new file and commit
Just simply create a new file, for example file1.txt and then:
```
git add file1.txt
git commit -a -m "Add file1.txt"
git push origin master
```

# Testing 2 users simultaneously making changes
In this example, user1 wants to edit file1.txt and user2 adds a new file2.txt but also makes some changes to file1.txt.
user1 has pushed first the changes to the repository:
```
gedit file1.txt (make the changes)
git commit -a -m "Modify file1.txt"
git push origin master
```

Now user2 has created a new file2.txt and wants to modify also the same file1.txt:
```
gedit file2.txt (write something)
gedit file1.txt (modify the original file)
git commit -a -m "Add file2.txt. Modify file1.txt"
git push origin master
```
Because user2 does not have the latest changes pushed by user1, you can see the message:

```
To https://github.com/ramonvp/git_test.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/ramonvp/git_test.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
To fix this situation, user2 has first to download the latest changes, which can be done by:
```
git pull
```
However, what happens is, since both users modified the same file, we now have a CONFLICT that shows the following message:
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/ramonvp/git_test
   f482148..84724af  master     -> origin/master
First, rewinding head to replay your work on top of it...
Applying: Add file2. Edit file1
Using index info to reconstruct a base tree...
M	file1.txt
Falling back to patching base and 3-way merge...
Auto-merging file1.txt
CONFLICT (content): Merge conflict in file1.txt
error: Failed to merge in the changes.
Patch failed at 0001 Add file2. Edit file1
The copy of the patch that failed is found in: .git/rebase-apply/patch

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
```
Let's see what is the conflict about:
```
cat file1.txt 
<<<<<<< 84724afbe4c512eca04184f2832a1625abbd3eaa
¡Hello Git!
=======
Hello Git!!
>>>>>>> Add file2. Edit file1
```
Alright then, since user1 decided to add an exclamation mark before, then we decide to add a second exclamation mark also. This will fix the conflict. user2 must manually erase the lines with symbols *======* and *>>>>>* to able to proceed.
```
gedit file1.txt (fix the file)
git commit -a -m "Fix conflict"
```
However, we cannot do right now a *git push* because we are in the middle of a rebase operation. We need to cancel it and then we can push the changes:
```
git rebase --skip
git push origin master
```
# Creating a pull request.
I first forked the project from Julia (https://github.com/easyrobotics/RM_Class1.git) using the button from the GitHub web page.
I then cloned the repo in computer, added a new text file and commit and pushed back to my repo.
Then, I pressed the button "Create pull request" from my repository directly on GitHub page. This creates a pull request for Julia to accept my changes in his repository.

