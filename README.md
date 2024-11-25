# Title

This runs standard git release on the shared action repositories

### Team Ownership - Product Owner

Automation Team

## How to Use

Create a new release in the `.github/workflows` folder, as described in the 
[GitHub Documentation](https://docs.github.com/en/actions/writing-workflows/quickstart).  Add the following code, 
or something like it:

```yaml
env:
  BRANCH_NAME: ${{github.ref_name}}
  TRUNK_BRANCH_NAME: 'main'

jobs:
  release:
    name: Release
    runs-on: ubuntu-24.04
    if: github.repository_owner == 'ikmdev'
    steps:
      - name: Verify Branch
        if: env.BRANCH_NAME != env.TRUNK_BRANCH_NAME
        run: |
          echo "ERROR: Attempting to release from branch ${{env.BRANCH_NAME}}. Release from ${{env.TRUNK_BRANCH_NAME}} branch only"
          exit 1

      - name: Release IKMDEV Code
        uses: ikmdev/maven-release-action@main
        with:
          ikmdevops_pat: ${{secrets.IKMDEVOPS_PAT_TOKEN}}
          github_token: ${{secrets.GITHUB_TOKEN}}
          ossrh_username: ${{secrets.OSSRH_TOKEN_USER}}
          ossrh_token: ${{secrets.OSSRH_TOKEN_PASS}}
          gpg_key: ${{secrets.GPG_KEY}}
          gpg_passphrase: ${{secrets.GPG_PASSPHRASE}}
```

## Issues and Contributions
Technical and non-technical issues can be reported to the [Issue Tracker](https://github.com/ikmdev/maven-release-action/issues).

Contributions can be submitted via pull requests. Please check the [contribution guide](doc/how-to-contribute.md) for more details.