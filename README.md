# Title

This runs standard git release on the shared action repositories

### Team Ownership - Product Owner

Automation Team

## How to Use

Create a new release in the `.github/workflows` folder, as described in the 
[GitHub Documentation](https://docs.github.com/en/actions/writing-workflows/quickstart).  Add the following code, 
or something like it:

```yaml
name: Release Workflow

run-name: "[${{inputs.release_type}}] Releasing ${{ github.event.repository.name }} #${{github.run_number}}"

# Trigger workflow manually
on:
  workflow_dispatch:
    inputs:
        release_type:
            description: 'Release Type'
            required: true 
            type: choice
            options:
                - -- SELECT --
                - Major
                - Minor
                - Patch
            default: -- SELECT --

env:
  BRANCH_NAME: ${{github.ref_name}}
  TRUNK_BRANCH_NAME: ${{github.event.repository.default_branch}}

jobs:
  release:
    name: "[${{inputs.release_type}}] Release"
    runs-on: ubuntu-24.04
    if: github.repository_owner == 'ikmdev'
    steps:
      - name: Verify Release Type Input
        if: inputs.release_type == '-- SELECT --'
        run: |
            echo "ERROR: Please enter release type"
            exit 1

      - name: Verify Branch
        if: env.BRANCH_NAME != env.TRUNK_BRANCH_NAME
        run: |
          echo "ERROR: Attempting to release from branch ${{env.BRANCH_NAME}}. Release from ${{env.TRUNK_BRANCH_NAME}} branch only"
          exit 1

      - name: "[${{inputs.release_type}}] Release Shared Action"
        uses: ikmdev/git-shared-action-release-action@main
        with:
           ikmdevops_pat: ${{secrets.IKMDEVOPS_PAT_TOKEN}}
           github_token: ${{secrets.GITHUB_TOKEN}}
           release_type: ${{inputs.release_type}} 

```

## Issues and Contributions
Technical and non-technical issues can be reported to the [Issue Tracker](https://github.com/ikmdev/maven-release-action/issues).

Contributions can be submitted via pull requests. Please check the [contribution guide](doc/how-to-contribute.md) for more details.