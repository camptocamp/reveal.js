# reveal.js

This is [reveal.js] with Camptocamp theme.

## Contributing

`upstream` branch must be a direct ancestor of the upstream main branch: it can be behind the upstream main branch, but cannot diverge from it. `main` branch must be ahead of `upstream` branch: each time `upstream` is synced with upstream, `main` has to be rebased.

### Prerequisites

Add upstream remote repository:

```
git remote add upstream git@github.com:hakimel/reveal.js.git
git fetch upstream
```

### Rebasing on upstream main branch

```
git checkout upstream
git pull --tags --ff-only upstream master
git push
git checkout main
git rebase upstream
npm-clean install
npm run build
git commit --amend
git push --force-with-lease
```

### Releasing a new version

```
git checkout <upstream-tag>
git cherry-pick upstream..main
npm clean-install
npm run build
cat package.json | jq -r '.version |= (split(".") | map(. |= tonumber) | .[2] |= . * 100 + 1000 + 1 | join("."))' | tee package.json
git add dist/ package.json
git commit --amend
tag="$(jq -r .version package.json)"
git tag "$tag"
git push origin "$tag"
```

[reveal.js]: https://revealjs.com/
