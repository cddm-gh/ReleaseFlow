## Commitizen Steps:

- Install the package `npm install --save-dev commitizen`
- `npx commitizen init cz-conventional-changelog --save-dev --save-exact`
- Add a new script to package.json:

```javascript
"scripts": {
    "commit": "cz"
}
```

## Adding commit lint and husky to enforce rules

- `npm install --save-dev @commitlint/config-conventional @commitlint/cli`
- `npm install husky --save-dev`

### Configure commit lint

- Create a `commitlint.config.js` and add the following:
  `module.exports = {extends: ['@commitlint/config-conventional']}`

To lint messages before they are committed, we need to use the commit-msg hook from Husky by running the following commands:

```javascript
# Activate hooks
npx husky install
# Add hook
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

You can add husky install as an npm prepare script, however, this step is optional. husky install will ensure that every developer using this repo will install Husky Hooks before using the project:

```javascript
...
"scripts": {
...
  "prepare": "husky install"
}
```

We’ll still use git commit to make our commits follow the convention described earlier. If there is a mistake in the git message, commitlint will raise the following errors:

```javascript
git commit -m "This is a commit"
⧗   input: This is a commit
✖   subject may not be empty [subject-empty]
✖   type may not be empty [type-empty]

✖   found 2 problems, 0 warnings
ⓘ   Get help: \[https://github.com/conventional-changelog/commitlint/#what-is-commitlint\](https://github.com/conventional-changelog/commitlint/#what-is-commitlint)

husky - commit-msg hook exited with code 1 (error)
```

## Final workflow for managing releases

To manage your releases, you can follow the workflow listed below:

1. Create your features and commit them. If commit messages aren’t following convention, commitlint will raise errors.
2. Execute the npm run commit in the command line to make a commit with Commitizen.
3. Run npm run release to create a changelog and a semantic versioning-based release.
