# Plugin release workflows

## Publish release workflow

This workflow will publish a release corresponding to the given tag.

You can use this workflow using the following Github Actions configuration.
The only requirement is the presence of the `glpi-project/tools` in the composer (dev) dependencies of the plugin.
This workflow will require the `contents: "write"` permission to be able to create a release.

```yaml
name: "Publish release"

on:
  push:
    tags:
       - '*'

jobs:
  publish-release:
    permissions:
      contents: "write"
    name: "Publish release"
    uses: "glpi-project/plugin-release-workflows/.github/workflows/publish-release.yml@v1"
    with:
      # The name of the tag.
      # Default: "${{ github.ref_name }}"
      tag: ""

      # The name of the release.
      # Default: "${{ github.ref_name }}"
      release-name: ""

      # The release description.
      # If the repository contains a `CHANGELOG.md` file, the default description will contain a link to this file,
      # otherwise, the description will be empty.
      release-body: ""
```

By default, the release is created using the workflow `GITHUB_TOKEN`. To use another token, see [Authentication](#authentication).

```yaml
jobs:
  publish-release:
    permissions:
      contents: "write"
    uses: "glpi-project/plugin-release-workflows/.github/workflows/publish-release.yml@v1"
    secrets:
      GLPI_PROJECT_BOT_PRIVATE_KEY: "${{ secrets.GLPI_PROJECT_BOT_PRIVATE_KEY }}"
```

## Automatically tag new version workflow

This workflow can be used to automatically create a new tag when the plugin version in its `setup` file is updated.

```yaml
name: "Automatically tag new version"

on:
  push:
    branches:
      - "main"
    paths:
      - "setup.php"

jobs:
  auto-tag-new-version:
    name: "Automatically tag new version"
    uses: "glpi-project/plugin-release-workflows/.github/workflows/auto-tag-new-version.yml@v1"
    secrets:
      GLPI_PROJECT_BOT_PRIVATE_KEY: "${{ secrets.GLPI_PROJECT_BOT_PRIVATE_KEY }}"
```

A token other than the workflow `GITHUB_TOKEN` is required, otherwise the tag push would not trigger other workflows. See [Authentication](#authentication).

## Authentication

Both workflows accept, in this order of precedence:

1. `bot-token` secret: a token (PAT or GitHub App token), used as is.
2. `bot-client-id` input and `bot-private-key` secret. Used by a GitHub App with the `contents: write` permission.  
    They default to the `GLPI_PROJECT_BOT_CLIENT_ID` variable and the `GLPI_PROJECT_BOT_PRIVATE_KEY` secret, which must be passed by name.
3. The workflow `GITHUB_TOKEN` (publish release workflow only).

`github-token` is a deprecated alias of `bot-token`, used when `bot-token` is not set.
