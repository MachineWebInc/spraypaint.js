# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.10.22-mt.3] - 2026-07-24

### Added

- Publish the maintained fork as `@machinewebinc/spraypaint`.
- Document upstream synchronization and release procedures.
- Publish releases from GitHub Actions using npm trusted publishing.

### Changed

- Merge the latest `graphiti-api/spraypaint.js` changes.
- Use a committed Yarn lockfile for reproducible builds.
- Run CI and package builds on Node.js 24.
- Preserve the unscoped `spraypaint` filename and global name for UMD builds.
