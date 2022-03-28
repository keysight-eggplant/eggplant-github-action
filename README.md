[![Eggplant Runner Build](https://github.com/TestPlant/eggplant-github-action/actions/workflows/eggplant-runner-demo.yml/badge.svg)](https://github.com/TestPlant/eggplant-github-action/actions/workflows/eggplant-runner-demo.yml)

# Eggplant Runner in CI/CD with GitHub Actions

Eggplant Runner currently provides "Run Test Config" as its main action.

## Using run-test-config.yml in your workflow

In order to use the Eggplant Runner with GitHub Actions, you need to add this to your GitHub Workflow:

```yaml
name: Eggplant Runner Build

# Configure the trigger of your workflow
# on:
#  push:
#    branches:
#      - main
#  pull_request:
#    branches:
#      - main

jobs:
  Run-Test-Config:
    strategy:
      max-parallel: 1
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    runs-on: ${{ matrix.os }}
    name: Run Test Config
    steps:
      - run: echo "Trigger event.. ${{ github.event_name }}. Runner OS.. ${{ runner.os }}."
      - name: Run Test Config
        uses: TestPlant/eggplant-github-action/actions/run-test-config@main
        with:
          serverURL: "" # Required. Details below
          testConfigID: "" # Required. Details below
          clientSecret: "" # Required. Details below
```

## Inputs

### `serverURL`
**Required** The URL of the DAI server, e.g. `http://localhost:8000`.

### `testConfigID`
**Required** The ID of the test config that you want to run, e.g. `09c48b7d-fc5b-481d-af80-fcffad5d9587`.

### `clientSecret`
**Required** The client secret to use to authenticate with the DAI server, e.g. `e9c15662-8c1b-472e-930d-aa0b11726093`.<br />
             Alternatively, you could set a repo secret in `GitHub Settings > Secrets > Actions` and refer to it like below:<br />
             `clientSecret: "${{ secrets.DAI_CLIENT_SECRET }}"`.
             
### `clientID`
**Optional** The client ID to use to authenticate with the DAI server.<br />
**Default:** `client:dai:agent:integration`

### `requestTimeout`
**Optional** The timeout in seconds for each HTTP request to the DAI server<br />
**Default:** `30`

### `requestRetries`
**Optional** The number of times to attempt each HTTP request to the DAI server<br />
**Default:** `5`

### `backoffFactor`
**Optional** The exponential backoff factor between each HTTP request<br />
**Default:** `0.5`

### `pollInterval`
**Optional** The number of seconds to wait between each call to the DAI server<br />
**Default:** `5`

### `logLevel`
**Optional** The logging level<br />
**Default:** `INFO`

### `CACertPath`
**Optional** The path to an alternative Certificate Authority pem file<br />

### `dryRun`
**Optional** Dry Run mode only validates the parameters without executing a test config run. It does not require a connection to the DAI server.<br />
**Default:** `False`.


## Notes

1. This workflow .yml file needs to in the `.github/workflows` directory in your repository on GitHub.<br />
Reading: https://docs.github.com/en/actions/quickstart.


2. On `strategy: max-parallel: 1`: SUT(System Under Test) is locked for one test config run at a time.<br />
Hence, we can only do unilateral testing.


3. Eggplant Runner supports these OS: Linux, Windows, MacOS.


4. On referencing the action .yml script:
    - You may reference our `run-test-config.yml` directly from your workflow .yml by defining the `uses` step as below:<br />
    `uses: TestPlant/eggplant-github-action/actions/run-test-config@main`
    - Or, you could also clone it to your own repository and point the `uses` step to it instead. This would also allow you to modify your own copy of the action script to your liking/needs.
