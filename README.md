# README #

This is the showcase of "GitHub flow" - a flow for continuous delivery projects.

## Why not use Git flow? ##

Git flow is perfect for versioned software. Not so pefect for continuous delivery though. In the first dozen commits I will demonstrate the common pitfall with this flow. Just follow the commit messages and see Pull requests (make sure you adjust the filter to see merged pull requests too).

## GitHub flow ##

It is very simple and quick flow, and being simple it is also extremely flexible. You can convert it literally into any, possibly more elaborate flow mid-way should you need it. The point is, you don't need elaborate flow everyone is scared of.

### What are we aiming to solve: ###
- feature downtime
- code contamination
- big scary merge

> I will call all individual branches "feature branches", because in this flow there is no significant difference between `hotfix` and `feature` branch, and that is actually one of its advantages.

### The idea ###

- The main branch is `main`. It is the only source of truth and the actual state of your app. You should always start your `feature`, `bugfix` or `release` branches from the `main` branch.

- The developer is always responsible for his own branch, form start to finish. Ideally, you shouldn't let others merge your feature in your stead, but I guess if you open Pull request your teamlead can do it for you as an exception (he will need to get accustomed with your feature and its test results).

> If you are familiar with Git flow, consider `main` as your `development` branch with just one difference. The `main` branch should always be ready to roll into production. There should be no state when the main branch contains untested, unfinished work and can't be deployed. The advantage of this approach is you don't need to overcommunicate to make sure you can deploy, and there is no bits of crucial information outside of the repository - everything is right there.

### Staging branch ###

`Staging` branch is optional. We use it to simplify testing, so that our tester team does not need to re-deploy each individual feature branch to test them. We merge our feature branches into `staging` to have them tested, nothing more than that.

> `Staging` branch should never be merged into main. The reason is because it is "dirty" branch that can contain bugs. It only becomes "clear" for short time when everything is tested and the new features are not accepted (which creates delays and is exactly what we want to avoid). You should use `release` branches when you need something like that, for more info see below.

### The main sequence ###

1. Start from `main`. Always.

2. Commit your changes into your branch.
*Never commit directly into `main` or other common branches.*
You can merge `main` into your branch any time if you need latest commits from there.

3. Open Pull request when you need a review or advice (even on unfinished feature). *The target branch is `main`.* Since you are the one responsible for your branch, it is safe to open pull request even when your feature is not ready. Only you can merge it, and you know when you are done.

4. Merge your branch into `staging` when your feature is ready to be tested.
*Do not close your branch on this merge.*

5. Update your feature branch if you get bug reports. Repeat step 4 to have them re-tested.

6. Finally, after receiving positive test results, merge your branch into `main` and close it. Done!

### What if I need the latest code which is not in the `main` yet? ###

In this flow, this should rarely happen, because nothing hinders feature branches from being merged as soon as it is ready. But should you need some other's (unfinished or not tested!) work, you can merge other dev's branch into yours.

This comes with the consequences! Since you are using somebody's unfinished work, you are now responsible for keeping it up to date by merging new commits from that branch into yours, and you must keep an eye on any bug reports in it.

### Why don't just use staging branch? ###

When you start from `staging` or merge it to your branch, you can't clearly see which branches you merged into yours and what is their current status. Getting somebody's unfinished work is already bad enough and will require additional care, but getting unknown number of changesets from God-knows-who is just shooting your own foot. In the end, it is extra work to figure out what branches are there, and communicate with their owners to make sure they are clear.

That's why it is important that you explicitly merge other's branch into yours, to avoid all this hassle. And a good incentive to only get what you really need.

### Release branches: what if we need multiple features released at once? ###

In this case, create a `release/Multiple-or-big-feature-release` branch from main. Merge your feature branches there. As always, commit your changes into your own feature branch, don't commit directly into the `release` branch. When you finished your part, merge your branch into `release`. When you need it tested, deploy `release` branch to test server, or merge it into `staging` and deploy.

When everything is completed and tested, you can merge `release` branch directly into main, or merge all features into main separately. How to do it depends on you.

It will be easier to *review* each feature separately, since you don't get a bunch of irrelevant code. But, if you decide otherwise, that's fine too. The reason we can do this is because the `release` branch is short-lived and does not inherit any irrelevant, old or forgotten changesets that `staging` may contain.

It is important that `release` branches are deleted when they merge into main, and started again from main when you need them next time. This prevents them from becoming "dirty". Actually, this is the reason I think `staging` should also be deleted and started from main periodically. Nobody wants forgotten changesets to ruin the day.

### Don't be afraid to merge whenever you need to ###

It is common fear of "big scary merge" with conflicting changes that plagues developers and makes them refrain from merge process as long as they can. In this flow, merges are usually small, clean and easy (well, I guess it depends on how you divide your work into features, it is always good to keep things small and clean). Forget this fear and merge whenever you need. If you get a merge conflict, it is always better when the changes are still small.

If there is a merge conflict when you merge into staging, that means you will potentially get the same conflict when you are going to merge into main. In this case, it may be beneficial to merge the conflicting branch into yours to solve it right there in your branch and avoid merge conflict later. Whether to do this depends on the situation, but I usually prefer to keep things separate. I can solve the merge conflict again when I merge into main, no problem, since I already know how.

### Can we go without staging branch? ###

Yes, and actually, we should. Normally, a tester would pull your feature branch, deploy it to his own server, and test it. And do it for every feature. If there are concerns that these features may affect eachother, a tester or developer would create `test` or `release` branch from main, merge all features there, deploy and test them alltogether. As you can see, no `staging` branch is required.

However, this assumes relatively high tech knowledge and access permissions for your testers, and individual test rig for each. Since we only have one test/staging server, and our testers can't deploy the code on their own, we use `staging` branch which represents the staging server.

### Evolving into something else ###

Like I said, this flow can be transformed into any other flow when necessary. For example, you can make separate `production` branch. Remember I said before, that the `main` branch should always be ready for production? This is the main idea of continuous delivery. Well, sometimes you just can't be that accurate. In this case, you can create separate `production` branch to deploy from there. Actually, you can create infinite number of pre-production branches, like `test`, `acceptance`, `stable` and so on. See, this flow can be transformed even into the classic Git flow. But, do you really need all that overcomplication when you already know how to make it with minimal resources?
