# Alpheios Developer Documentation

This repository contains the Alpheios Developer Documentation. The issues list is the place to discuss major development issues such as refactoring, 
technologies decisions, etc.

This README document summarizes our current development practices.

## Quick Links

[Source code documentation](https://github.com/alpheios-project/documentation/blob/master/development/source-code-documentation.md)
[Git workflow](https://github.com/alpheios-project/documentation/tree/master/development/git-workflow)

## Development Practices

The summary of best development practices is provided in the [Development Practices Summary](https://github.com/alpheios-project/documentation/tree/master/development)

## GitHub Workflows

Actively developed Alpheios code repositories should adhere to the following workflows. (Not all legacy repositories have been migrated.)

### Main Branches

* the **master** branch is the development branch
* the **qa** branch contains code currently undergoing QA in preparation for release
* the **production** branch contains the current release

See also the [Git Workflow Document](https://github.com/alpheios-project/documentation/tree/master/development/git-workflow)

### Development 

When you are fixing a bug or starting a new feature, create a branch off of master and do your development work in there as usual. 
Name the branch according to the following scheme:

For issues in the current repository:

```
i<issue-number>-[description]
```

If the issue is hosted by another repository (in which case ideally there would be a companion issue entered in the current repository but
it could be overlooked):

```
i<issue-number>-<repo-name>-[description]
```

**When possible use conventional commits.** For at least one commit in your development branch, run `npm run conventional-commit` and follow the prompts to specify the type of change you are making.

After your work is completed, and the PR approved, merge your changes to master.

## QA Builds

1. If the `qa` branch is not created, create it, and push it to GitHub
2. merge `master` to the the `qa` branch.
3. resolve any merge conflicts
4. make sure dependencies are all up to date per the repository practice
5. run `npm tagged-commit`
6. run `git push && git push --tag`

When the tagged build passes tests, if the repository contains an npm package, it will be automatically published to npm under the **qa** tag.

## Production Builds

1. If the `production` branch is not created, create it, and push it to GitHub
2. merge `qa` to the the `production` branch.
3. resolve any merge conflicts
4. make sure dependencies are all up to date per the repository practice
5. run `npm tagged-commit`
6. run `git push && git push --tag`

When the tagged build passes tests, if the repository contains an npm package, it will be automatically published to npm under the **rc** tag.

