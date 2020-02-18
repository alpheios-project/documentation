# Git workflows

# Standard workflow

This is a workflow that will be used for development of a new feature  most often. Please see details on a diagram below.

![Standard workflow diagram](https://raw.githubusercontent.com/alpheios-project/documentation/master/development/git-workflow/git-std-worklow-proposal.svg?sanitize=true)

When a new feature development is started, a separate feature branch is created for it. This branch will contain code of this feature only. Once development of this feature and its dev testing are complete, a feature branch is merged into master. A feature branch can be deleted after the merge.

A feature is moved to QA from a `master` branch. If `master` contain other new features at the moment, all those feature will go to QA and will be tested simultaneously. If any issues are found during QA testing, a separate QA branch should be created to implement those fixes. That branch will be merged back to a QA branch after fixes are completed. It can be deleted afterwards as it is not needed any more.

Changes approved by QA are merged into a `release` branch. At that point of time we also need to merge `release` branch to `master`. This will bring `master` up to date with the latest fixes. A `release` branch serves as an accumulator for changes that are fully tested and ready for production. When we decide to make a production release we take changes from the `release` branch and merge them to `master`.

# Fast track workflow

This workflow is used on special occasions when do have some features in QA testing already but we want one or several other features to be released before them. For that we will use a special `qa-ft` branch that will isolate fast track feature from features already in a `qa` branch.

This workflow is more complex and should be used only when it is absolutely necessary.

![Fast track workflow diagram](https://raw.githubusercontent.com/alpheios-project/documentation/master/development/git-workflow/git-fast-track-worklow-proposal.svg?sanitize=true)

The fast track workflow differs from the regular one in the following:
* We do not merge changes from master to a fast track feature development branch and we do not merge a fast track branch to master before merging a fast track branch to QA. At that moment `master` may contain other features already developed. We don't want them to be combined with the fast track features. We want to isolate the fast track features so that they will be tested and released on their own.
* We merge changes from the fast track feature development branch into a special fast track `qa-ft` branch, not to the regular `qa` branch. This is to isolate fast track features from the rest.
* After a fast track QA testing is completed, a `qa-ft` branch is merged to a `release`. Then a `release` branch needs to be merged to the regular `qa` branch to add fast track changes to what is currently in QA. We also need merge `release` to `master`. At that moment a fast-track QA branch is not needed and can be removed.

# Version changes

Certain waypoints of the workflow result in a version update. A version is updated when a development of a feature is completed and a feature branch is merged to `master`, or when a set of changes either goes to QA or is merged to `release` or `production` branches.

Each version change event should also result in a new code deployment. We can use functionalities of Lerna and Travis to implement a version change and a deployment of build artifacts. 

# Workflow waypoints

Below is a list of all important workflow wayponts that cause a version change and/or require some other special handling. Each case list what actions must be taken during the waypoint.

An automatic pushing of changes during a Lerna version change is disabled currently. As we have not tried the procedures described here yet it is good to have more control over how things happen. Later, if we decide that everything can go full automatic, we can enable Lerna to push changes along with a version change.

Here is a list of major waypoints:

## A start of development of a new feature

1. Create a new branch out of `master`. This branch can have an arbitrary, but unique name. Do all development of a feature, its testing, and bug fixing in this branch.

## A new feature development is completed

1. Merge all changes from `master` to a feature branch. This will make sure that a feature branch incorporates all recent updates. Make sure this merge does not break a functionality of a feature.
2. Merge changes from a feature branch to `master`.
3. Increase a version number according to the schema described below. Create a new commit that will incorporate a version change. Tag it with a `vMajor.Minor.Patch-dev.PrereleaseVersion` tag (e.g. `v1.2.3-dev.0`). All this can be done with one of the Lerna npm tasks (please see details below).
4. Push changes to `master`. This will trigger a new deployment (deployment details are TBD).

The version is increased according to what set of features was developed. Let's say that the current version of the code in master is `1.2.3-dev.3`. Then the version will be changed as:
* If a feature introduced is a major one: `1.2.3-dev.3`->`2.0.0-dev.0`. Use `version-set-major-dev` lerna npm task to make this change.
* If a feature introduced is a minor one: `1.2.3-dev.3`->`1.3.0-dev.0`. Use `version-set-minor-dev` lerna npm task for this.
* If a feature introduced is a patch one: `1.2.3-dev.3`->`1.2.4-dev.0`. Use `version-set-patch-dev` lerna npm task to do the version update.

## Bring changes to QA

Changes go to QA from `master`, with the exception of the fast track workflow. A fast track workflow waypoints will be described separately. 

1. Merge a `master` branch to`qa`.
2. Update the package version in `qa` by changing the prerelease postfix from `dev` to `qa` and set a prerelease version number to 0. The version number itself should not be changed: `1.2.3-dev.3`->`1.2.3-qa.0`. Create a commit with a version change and tag it by adding a `v` prefix to the version number. All this can be done by the `version-increase-prerelease-qa` Lerna npm task.
4. Push changes to `qa`. This will trigger a deployment (deployment details are TBD).

## Fix issues found in QA

1. Create a separate branch for the fix. It can use a `qa-fixes-vMajor.Minor.Patch-qa.PrereleaseVersion` schema (i.e. `qa-fixes-v1.2.3-qa.0`) to designate for what version those changes are. Merge `qa` into that branch.
2. Fix bugs within that branch and test the fix.
3. Merge changes from that branch back to the `qa`. 
4. Increase a qa prerelease version number: `1.2.3-qa.0`->`1.2.3-qa.1`. Create a commit with a version change and tag it by adding a `v` prefix to the version number. All this can be done by the `version-increase-prerelease-qa` Lerna npm task.
5. Push changes to `qa`. This will trigger a deployment (deployment details are TBD).

## Release after a QA approval

1. Merge changes from the `qa` to the `release` branch.
2. Change the prerelease suffix from `qa` to `rel` and set a prerelease version number to zero: `1.2.3-qa.1`->`1.2.3-rel.0`. Create a commit with a version change and tag it by adding a `v` prefix to the version number. All this can be done by the `version-increase-prerelease` Lerna npm task.
3. Push changes to a `release` branch. This will trigger a deployment (deployment details are TBD).
4. Merge changes from the `release` branch to `master`. Do not change the `major.minor.patch` version number in `master` because it is the same or higher as in `release`. Increase, however, a prerelease version: if master version is `1.2.4-dev.0` and a release version `1.2.3-rel.0`, change master version `1.2.3-dev.1`.

## Release to production

1. Merge `release` to `production`.
2. Update the version by removing the prerelease postfix. The `major.minor.patch` version number should stay the same: `1.2.3-rel.0`->`1.2.3`. Create a commit with a version change and tag it by adding a `v` prefix to the version number. All this can be done by the `version-graduate` Lerna npm task.
3. Push changes to `release`. This will trigger a deployment (deployment details are TBD).

## Fast track workflow: fast track QA branch is merged to the regular QA one

This merge requires a complex version change because it requires the version number to reflect the change introduced by both a feature set currently in QA and a feature set of a fast track version. The resulting version has to be calculated manually as there seems to be no way to automate it. We should think about how to make it easier on us.