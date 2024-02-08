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
