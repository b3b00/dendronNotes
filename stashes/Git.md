---
id: 76127ae3-29f8-41de-9ed7-a342fbb81cef
title: Git
desc: Git tips
updated: 1769965994
created: 1769795609
---

# tags

## Delete

Local
```bash
git tad -d mytag
```

Remote
```bash
git tag origin mytag - - delete
```
______________

# commits

## undo

```
git reset HEAD~1
``` 
An d set an alias :

```bash
git config --global alias.undo-commit 'reset HEAD~1'
# or the hard way
git config --global alias.undo-commit-hard 'reset --hard HEAD~1'

```
______________

# branches

## rename

### local

```bash
git checkout old_name
git branch -m new_name
```

### remote

There is no short way to rename remotely. You have to remove and push new.

```bash
# Delete the old branch on remote
git push $remote --delete $old_name

# Or shorter way to delete remote branch [:]
git push $remote :$old_name

# Prevent git from using the old name when pushing in the next step.
# Otherwise, git will use the old upstream name instead of $new_name.
git branch --unset-upstream $new_name

# Push the new branch to remote
git push $remote $new_name

# Reset the upstream branch for the new_name local branch
git push $remote -u $new_name

```