# Auto-Increment Semantic Versioning Numbers

## Purpose

This is a reusable Github workflow to increment [semantic version numbers](https://semver.org/) based on [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/). 


## How it works

First, it tries to find and download the latest `version.txt` artifact from your Github repository. If none is found, it assumes a version of "1.0.0.0".

Next, a version bump script checks what changes have been made and then updates the version number accordingly, ensuring that the version reflects the significance of the changes. This Python script first fetches all commit messages since the last push and analyzes the changes:

* It goes through each commit message one by one.
* **Major Change**: If any message indicates a "BREAKING CHANGE" or contains the special marker `!:`, it decides that the update should be a major version bump and stops checking further messages.
* **Minor Change**: If a message starts with `feat:`, meaning it adds a new feature, it marks the update as minor.
* **Patch**: If a message starts with `fix:`, meaning it fixes a bug, it marks the update as a patch, but only if a minor update hasn't already been applied.

Based on the decision from the messages, the version number is updated:

* For a **major** update, the major version number is incremented by one, and all minor numbers (minor, patch, and revision) are reset to zero.
* For a **minor** update, the minor number is incremented by one, and the patch and revision numbers are reset to zero.
* For a **patch** update, only the patch number is incremented by one, and the revision is reset to zero.
* If none of the above conditions are met, it simply increments the revision number.


## How to use

In your Github repository, create a workflow that automatically runs on every push by creating a file `.github/workflows/lf-autoinc-semver.yml` with this content:

```
name: 'Linuxfabrik: Auto Versioning on Push'

on:
  push:
    branches:
      - 'main'

# modify the default permissions granted to the GITHUB_TOKEN
permissions:
  contents: 'read'

jobs:
  versioning:
    uses: 'Linuxfabrik/autoinc-semver/.github/workflows/autoinc-semver.yml@main'
    secrets: 'inherit'  # when within the Linuxfabrik organization
```
