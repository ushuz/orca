# ushuz-build

Fork operations branch for [ushuz/orca](https://github.com/ushuz/orca), a
minimum-divergence fork of [stablyai/orca](https://github.com/stablyai/orca).

This is an orphan branch. It shares no history with the code branches and holds
only the build and release machinery, so upstream churn can never conflict with
it. It is the repository default branch for two reasons: GitHub fires
`schedule:` triggers only from the default branch, and keeping the default off
`main` means none of upstream's ~25 inherited workflows can run in this fork.

## Ref model

- `main` — byte-identical mirror of `upstream/main`, never committed to.
- `ushuz/vX.Y.Z` — one branch per upstream stable base, holding only fork
  customizations rebased onto that tag. The branch name records the base, so
  nothing else has to track it.
- `vX.Y.Z-uN` — release tag. `N` is the fork iteration against that base.
- `ushuz-build` — this branch, never rebased.

Old `ushuz/*` branches are disposable: release tags pin every commit that ever
shipped.

## Versioning

Both artifacts track their own upstream value at the base commit, plus the fork
iteration:

- desktop — upstream `package.json` version, suffixed `-uN` (`1.4.185-u1`)
- iOS — upstream `mobile/app.json` `expo.version` unchanged, with `N` as the
  build number, because Apple requires numeric marketing versions

`-uN` is a semver prerelease identifier, which is deliberate: an app whose own
version is a prerelease opts itself into the prerelease update feed
(`src/main/updater.ts`, `includePrerelease`). Build metadata (`+uN`) would not,
and would also compare equal between iterations, so updates would never fire.

## Overrides

Fork customizations live in `ushuz/*` branches as ordinary commits. Everything
needed to retarget an upstream build — release feed, bundle identifiers,
versions — is applied by the release workflow at build time and never committed,
so the code branches stay free of fork plumbing.

## Status

Scripts and workflows land in following commits.
