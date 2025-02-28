===
Git
===

Objects
=======

Objects are created whenever git command is executed.
They are immutable.

* annotated tag
* blob (binary large object)
* tree
* commit

In staging area (when git add), git takes contents of the file and puts it as a copy into a
file as blob in the objects directory. Blobs don't have names, they are just raw data.
The raw data is edited to have more info and ran through SHA, which always outputs 40 chars.
The file name of the file is not stored in blob object but in the tree. With this system,
multiple files with the same content are allowed to be stored. When the file name is changed,
git only needs to create new tree object with the new file name.The blob (raw data) doesn't
need to be changed.Files can be easily renamed without high cost.
Git uses compression algorithm to reduce total size of objects.

Branches
========

Branches are just pointers.

git diff
========

Comparing working area and staging area.

* git diff --staged
    * Comparing staging area and repo.

git reset
=========

Remove things out of rough draft (staging area).

git checkout
============

* git checkout commit
    * Git finds the commit, the tree that the commit references, and finds all subtrees or objects
      referenced by the tree and make the working directory identical to that snapshot.

git rm
======

Remove file from working dir and add to staging area.

git rebase
==========

Rewrite commits from one branch onto another.

git reflog
==========

Shows history of commits HEAD has referenced to.Use to undo rebase.
