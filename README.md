# Plugin release workflows

## Publish release workflow

This workflow will publish a release corresponding to the given tag.

You can use this workflow using the following Github Actions configuration.
The only requirement is the presence of the `glpi-project/tools` in the composer (dev) dependencies of the plugin.

```yaml
name: "Plugin release"

on:
  push:
    tags:
       - '*'

jobs:
  publish-release:
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
      # The access token used when the new tag is pushed.
      # This personal access token (PAT) is required to ensure that subsequent workflows that listen
      # to the tag push event will be triggered.
      # This PAT requires the `Content: read/write` permissions.
      github-token: "${{ secrets.AUTOTAG_TOKEN }}"
```
