# Preternatural Build Action

This GitHub Action allows you to run the Preternatural CLI build command on your repositories with support for a specific Xcode version.

## Features

- Run Preternatural `build` command
- Specify Xcode version for your build
- Configure build settings such as derived data path and configuration
- Version-specific caching of derived data

## Usage

### Inputs

- `xcode-version`: (Required) Xcode version to use.
- `derived_data_path`: (Optional) The path to the derived data folder.
- `build_all_platforms`: (Optional) Set to 'true' to build for all supported platforms. Defaults to 'false'.
- `configuration`: (Optional) Build configuration (debug or release). Defaults to 'debug'.

## Examples

### Build command with all platforms and custom derived data path

```yaml
- name: Run Preternatural Build
  uses: PreternaturalAI/github-action/preternatural-build@main
  with:
    xcode-version: '16'
    build_all_platforms: 'true'
    derived_data_path: '~/Desktop/derived_data'
    configuration: 'release'
```

## Full Workflow Example

Here's an example of how to use this action in a workflow:

```yaml
name: Preternatural Build
on:
  push:
    branches:
      - '**'

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
  
jobs:
  build:
    runs-on: macos-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v3
    
    - name: Run Preternatural Build
      uses: PreternaturalAI/github-action/preternatural-build@main
      with:
        xcode-version: '16'
        configuration: 'release'
```

This workflow does the following:

1. Triggers on push events for all branches.
2. Sets up the macOS environment.
3. Checks out the repository.
4. Runs the Preternatural build command with the following options:
   - Uses Xcode version 16
   - Builds for all platforms
   - Uses a custom derived data path
   - Builds in release configuration
   
The concurrency configuration ensures that only one workflow runs at a time for each branch, cancelling any in-progress workflows when a new one is triggered on the same branch.

## Notes

- The action automatically sets up the specified Xcode version before running the Preternatural command.
- Derived data is cached separately for each Xcode version to optimize build times.
- The action uses Homebrew to install the Preternatural CLI.
- For detailed information on Xcode version specification, refer to the [setup-xcode action documentation](https://github.com/marketplace/actions/setup-xcode-version).
