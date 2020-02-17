# Git workflows

# Standard workflow

This is a workflow that will be used most often during development of a new feature. This workflow is shown on a diagram below.

![Standard workflow](https://raw.githubusercontent.com/alpheios-project/documentation/master/development/git-workflow/git-std-worklow-proposal.svg?sanitize=true)

When a new feature is started to be developed, a separate feature branch is created for it. This branch will contain code of this feature only. Once the feature development and dev testing are complete, the feature is merged into master and a feature branch can be deleted.

A feature will go to QA with other features from the master branch. Whatever is in the master branch at a time when a new QA testing is initiated will be tested. If QA found any issues that need to be fixed those fixes will be made in a dedicated branch. That branch will be merged back to QA after fixes are completed; it can be deleted afterwards because it is not needed any more.

Changes approved by QA will be merged into a `release` branch. From the `release` branch they will be merged back to master so that the master will have all the fixes. The `release` branch serves as an accumulator for changes that are fully tested and ready for production. 
New features from the `release` branch can be merged to `production` and time.

# Fast track workflow

This workflow is used when we want one or several feature to be released before other features that are in QA already. In the fast track workflow we will use a separate QA branch to test the fast track features.

This workflow is more complex and should be used only when its increased complexity is fully justified by our needs.

![Fast track workflow](https://raw.githubusercontent.com/alpheios-project/documentation/master/development/git-workflow/git-fast-track-worklow-proposal.svg?sanitize=true)

The fast track workflow differs from the regular one in the following:
* We do not merge changes from master to a fast track feature development branch before this branch is merged to QA. We do this because we don't want changes from master that may contain other features to be merged into the fast track feature. We want to isolate a fast track feature so that it will go before all other features currently in development and/or testing.
* We merge changes from the fast track feature development branch into a special fast track QA branch, not to the regular QA branch. This is again to isolate fast track feature changes.
* After fast track QA testing is completed, fast track QA branch is merged to a `release` branch. After that we merge `release` to the regular QA branch to merge fast track changes that are approved already with the changes currently under tasting. We also merge `release` to `master` to propagate fast tracked features in there. When all this is done, a fast-track QA branch is not needed any more and can be removed.
