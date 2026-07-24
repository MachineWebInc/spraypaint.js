# Maintaining the MachineWebInc fork

This repository is a public fork of
[`graphiti-api/spraypaint.js`](https://github.com/graphiti-api/spraypaint.js).
It publishes the public npm package `@machinewebinc/spraypaint`.

## MachineWebInc changes

The fork currently carries these behavior changes:

- Find deeply nested associated records when assigning validation errors.
- Ignore attributes configured with `persist: false` during dirty checking.
- Compare `Array` and `Object` attributes by serialized value during dirty
  checking.

Keep fork-specific changes small, tested, and separate from upstream merges.
Submit generally useful fixes upstream whenever practical.

## Synchronize upstream

Configure the upstream remote once:

```sh
git remote add upstream https://github.com/graphiti-api/spraypaint.js.git
```

Create a synchronization branch from the current default branch:

```sh
git fetch upstream
git switch master
git pull --ff-only origin master
git switch -c chore/sync-upstream
git merge upstream/master
```

Resolve conflicts, run `yarn build`, and open a pull request. Merge upstream
history; do not rebase published release history.

## Prepare a release

1. Synchronize upstream when appropriate.
2. Update `CHANGELOG.md`.
3. Set the next version in `package.json`.
4. Run:

   ```sh
   yarn install --frozen-lockfile
   yarn build
   npm pack --dry-run
   ```

5. Open and merge a pull request containing the release changes.
6. Tag the merge commit with the exact package version prefixed by `v`:

   ```sh
   git switch master
   git pull --ff-only origin master
   git tag -s v0.10.22-mt.3
   git push origin v0.10.22-mt.3
   ```

The `Publish` GitHub Actions workflow verifies that the tag and package
versions match, rebuilds the package, and publishes it to npm with the `latest`
dist-tag. The explicit tag is required because npm treats `-mt.*` versions as
prereleases.

Never reuse a published npm version. Increment the MachineWebInc suffix for a
fork-only release. When incorporating a newer upstream release, use that
upstream version and restart the suffix at `mt.1`.

## npm trusted publishing

The npm package must trust this repository's `.github/workflows/publish.yml`
workflow. The workflow uses GitHub's OIDC identity and does not require a
long-lived `NPM_TOKEN`.

The first publication under a new npm package name must be completed by an npm
member of the `machinewebinc` organization. Afterward, configure this workflow
as the package's trusted publisher on npm.
