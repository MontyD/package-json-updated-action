# GitHub Action to detect changes in package.json version

This action outputs a variable 'has-updated' indicating that the package.json version was updated in the most recent commit.

## Outputs

### `has-updated`

Whether the package.json version has updated.

### `current-package-version`

The current package.json version

## Example usage

```
jobs:
  run-build-if-needed:
    runs-on: ubuntu-latest
    steps:
    - uses: MontyD/package-json-updated-action
      id: version-updated
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    - uses: actions/checkout@v1
      if: steps.version-updated.outputs.has-updated
      with:
        fetch-depth: 1
    - uses: actions/setup-node@v1
      if: steps.version-updated.outputs.has-updated
      with:
        node-version: '12.x'
    - run: npm install
      if: steps.version-updated.outputs.has-updated
    - run: npm run build
      if: steps.version-updated.outputs.has-updated
```