[![Tests](https://github.com/OMICRONEnergyOSS/oscd-menu-save/actions/workflows/test.yml/badge.svg)](https://github.com/OMICRONEnergyOSS/oscd-menu-save/actions/workflows/test.yml) ![NPM Version](https://img.shields.io/npm/v/@omicronenergy/oscd-menu-save)

# Shared Github Workflows

This repo contains a set of Github workflows which can be used by OpenSCD plugins to standardize the workflows. Meaning workflows needed by plugins can be centrally maintained.

Currently there are only three workflows:

# unit-tests.yml

- Creates a Playwright container
- Checks out the project
- Installs node modules
- Triggers ESLint (npm run lint)
- Triggers the unit tests (npm run test)

## Usage

```yml
name: Unit Tests
on:
  push:
    branches:
      - main

jobs:
  test:
    permissions:
      contents: read
    uses: OMICRONEnergyOSS/oscd-gh-workflows/.github/workflows/unit-tests.yml@main
```

# unit-and-vrt-tests.yml

- Creates a Playwright container
- Checks out the project
- Installs node modules
- Runs ESLint (npm run lint)
- Runs the unit tests (npm run test)
- Runs the Visual Regression Tests (vrt)

## Usage

```yml
name: Tests
on:
  push:
    branches:
      - main

jobs:
  test:
    permissions:
      contents: write
    uses: OMICRONEnergyOSS/oscd-gh-workflows/.github/workflows/unit-and-vrt-tests.yml@main
```

# update-screenshots.yml

- Creates a Playwright container
- Checks out the project
- Installs node modules
- Runs the Visual Regression Tests (vrt)
- Commits and Pushes the updated screenshots to the current branch.

## Usage

```yml
name: Update Screenshots
on:
  push:
    branches:
      - main

jobs:
  update-screenshots:
    permissions:
      contents: write
    uses: OMICRONEnergyOSS/oscd-gh-workflows/.github/workflows/update-screenshots.yml@main
```

# continuous-deployment.yml

Uses gh-pages to build a deployment bundle and push it to the "deploy" branch, assumes repo is configured to deploy the contents of this branch to github pages.

## Usage

```yml
name: Continuous Deployment
on:
  push:
    branches:
      - main

jobs:
  ci-cd:
    permissions:
      contents: write
      pull-requests: write
    uses: OMICRONEnergyOSS/oscd-gh-workflows/.github/workflows/continuous-deployment.yml@main
    secrets:
      gh_token: ${{ secrets.GITHUB_TOKEN }}
```

# release-please.yml

This is a two stage workflow. When code is merged into main, the workflow is triggered to scan the commits and based on the release-please configuration in the repository, may create a Pull Request for a new release - incrementing the project version number.

In the second stage, if a Release Please PR has been approved and merged into main, it will trigger two jobs:

1. Build the project and publish it two NPM using the new version number.
2. Build a release bundle and package it as a .zip file and .tar.gz file, which are then uploaded to the project releases page, for consumers to download into their distros.

## Usage

```yml
name: Release Please
on:
  push:
    branches:
      - main

jobs:
  release:
    permissions:
      contents: write
      issues: write
      pull-requests: write
      id-token: write
    uses: OMICRONEnergyOSS/oscd-gh-workflows/.github/workflows/release-please.yml@main
    secrets:
      npm_token: ${{ secrets.NPM_TOKEN }}
      gh_token: ${{ secrets.GITHUB_TOKEN }}
```

&copy; 2025 OMICRON electronics GmbH

## License

[Apache-2.0](LICENSE)
