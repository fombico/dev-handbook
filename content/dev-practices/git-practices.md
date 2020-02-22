---
title: "Git Practices"
date: 2020-02-21T14:39:34-05:00
draft: true
---

---

## Recommended Practices

### Small, stable commits

We prefer small, stable commits where possible. Small commits are easier to review and easier to undo.
Stable commits ensure the code compiles and tests pass if we checkout this commit. 
Also, squash your commits to avoid WIP commits in the history.   

### Continuous integration

Continuous integration from a code perspective is ensuring your code is up-to-date with master.
Ideally, you should be rebasing from master daily, if not more often.
You want to integrate sooner rather than later, to ensure code compiles and tests pass.
The longer you wait, the more likely the code you need to integrate with is moved, refactored, or deleted.

Continuous integration also means frequently merging to master. This means avoiding long-living feature branches. 
By merging frequently, other developers are also able to continuously integrate with your code.
Ideally, you should have _at most_ a week's worth of code to merge in - and having less than that is encouraged.    

### Pull rebase

We highly recommend to rebase flag when pulling:
```
git pull -r
```

Making use of rebasing has the following benefits:
- avoids merge commits
- keeps history as a clean single line
- makes it easier to git bisect

When working on a small team collaborating on the same branch, 
it will be common for your pushes to be rejected as someone else made a commit while you were working.

Merge commits (without rebase) are more suited for long-lived feature branches that integrate infrequently. 
However, since we advocate for small commits and continuous integration, rebasing is the way to go. 

### Force with lease

There are times when you need to force push a branch, thereby overwriting the remote branch with your local copy. 
If there are other commits in the remote branch by other members of the team, those changes will be lost. 
To avoid this scenario, consider making use of the `--force-with-lease` flag, which will reject your push.
In doing so, you will have the opportunity to incorporate their changes.

```
git push origin --force-with-lease <branch-name>
```

### Clean up branches

If you make branches - say for a pull request process, clean them up on remote once you are done merging.
You should avoid keeping old branches - they'll quickly go stale, no one remembers what they were working on, 
and it will clutter the repo. 

You should also clean up branches on your local machine. 
By using the prune command, you remove references to deleted remote branches:
```
git fetch --prune
``` 

---

## Common Commands Visualized

### git merge

You have a feature branch to merge into master.
 
![Git merge](/images/dev-practices/git-practices/git-merge.svg?width=500px)

On the master branch, you would run:
```
git merge <feature-branch>
```

Since there have been commits on master, B and C, a merge commit will be made.
The user will be asked to provide a commit message. 
The final result is commit G, with two parents.

![Git merge result](/images/dev-practices/git-practices/git-merge-result.svg?width=500px)

### git pull

You are on your local master branch, where you made a few commits. 
Your fellow developers have informed you they made changes to master.

![Git pull](/images/dev-practices/git-practices/git-pull.svg?width=500px)

You want to sync with the latest changes, so you ran:
```
git pull
```

The result is: 

![Git pull, no rebase](/images/dev-practices/git-practices/git-pull-no-rebase.svg?width=500px)

When the command ran, the latest commits were fetched from the remote master and __also__ merged into your local master.
As you can see, an additional merge commit, H, is created. As mentioned above, we __do not recommend__ this approach.

Instead use the rebase flag, as shown below:

```
git pull -r
```

The result would be:

![Git pull rebase](/images/dev-practices/git-practices/git-pull-rebase.svg?width=500px)

In this scenario, the latest commits were also fetched, but the local commits are replayed on top of master.
The end result is a clear line of commits. No merge commit is added.

__Note:__ on the replayed commits (E', F', and G'), the hashes are updated, while the date remains unchanged.

### git cherry-pick

In this scenario, you've identified a commit to apply to your branch. You run the following command:
 ```
git cherry-pick <commit-hash>
```

![Git cherry-pick](/images/dev-practices/git-practices/git-cherry-pick.svg?width=500px)

In the diagram above, the commit E is to be cherry-picked onto the green branch. 
The result is commit E' with a new hash. Commits D and F are not applied.

## Additional Resources

- [Bitbucket Merge Tutorial](https://www.atlassian.com/git/tutorials/using-branches/git-merge)
- [force-with-lease - Atlassian Developer Blog](https://blog.developer.atlassian.com/force-with-lease/)
