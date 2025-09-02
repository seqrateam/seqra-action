# Seqra GitHub Action

Run [Seqra](https://github.com/seqrateam/seqra) static analysis in your CI, generate a SARIF report, and optionally upload it to GitHub Code Scanning.


## Usage

> **Note:** The action expects **Linux x86\_64** runners.

### Quick Start

### Scan

```yaml
name: Seqra Analysis
on:
    workflow_dispatch

jobs:
  seqra:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository
        uses: actions/checkout@v4

      - name: Run Seqra code analysis
        uses: seqrateam/seqra-action@v1
```


### Scan and upload to GitHub code scanning alerts

```yaml
name: Seqra Analysis
on:
    workflow_dispatch

# Required for Code Scanning upload
permissions:
  contents: read
  security-events: write

jobs:
  seqra:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository
        uses: actions/checkout@v4

      - name: Run Seqra code analysis
        uses: seqrateam/seqra-action@v1
        with:
          upload-sarif: 'true'
          artifact-name: 'sarif'
```


### All Inputs

```yaml
name: Seqra Analysis
on:
    workflow_dispatch

# Required for Code Scanning upload
permissions:
  contents: read
  security-events: write

jobs:
  seqra:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository
        uses: actions/checkout@v4

      - name: Run Seqra code analysis
        uses: seqrateam/seqra-action@v1
        with:
            # Relative path under $GITHUB_WORKSPACE to the root of the analyzed project
            project-root: '.'

            # Should seqra-action upload sarif to GitHub Code Security
            upload-sarif: 'false'

            # Tag of seqra release
            seqra-version: 'v1.0.1'

            # Relative path under $GITHUB_WORKSPACE to your rules
            # By default it is empty, so seqra wil use builtin rules
            rules-path: 'security/myrules'

            # Name of uploaded artifact
            artifact-name: 'sarif'

            #Scan timeout
            timeout: '15m'
```


## Artifacts

After the job completes, you’ll find:

* A SARIF artifact named `sarif` (configurable) will be uploaded to the workflow run.
* If `upload-sarif: 'true'`, the SARIF is also sent to **Security → Code scanning alerts** in your repo.


## Permissions

* For **artifact upload**: default permissions are fine.
* For **Code Scanning upload**: add

  ```yaml
  permissions:
    contents: read
    security-events: write
  ```


## Troubleshooting

* **Monorepos:** You can analyze only the project you need using `project-root`.
* **Timeouts:** If the scan times out, increase `timeout` (e.g., `30m`).


## Changelog
See [CHANGELOG](CHANGELOG.md).


## License
This project is released under the [MIT License](LICENSE).

The [core analysis engine](https://github.com/seqrateam/seqra-jvm-sast) is source-available under the [Functional Source License (FSL-1.1-ALv2)](https://fsl.software/), which converts to Apache 2.0 two years after each release. You can use Seqra for free, including for commercial use, except for competing products or services.
