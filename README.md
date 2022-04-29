<img src="https://www.eggplantsoftware.com/hubfs/Branding/Keysight-Eggplant-Logo_RGB_full-color.svg" width="300px"/>

# Eggplant DAI integration build as GitHub Action

## Introduction

The [Eggplant DAI](https://www.eggplantsoftware.com/digital-automation-intelligence) integration build as GitHub Action are able to launches DAI tests from within a GitHub workflow pipeline. You can use it to continuously test your application's [model-based approach to testing](https://docs.eggplantsoftware.com/docs/dai-using-eggplant-dai/).  For more information about Eggplant, visit https://www.eggplantsoftware.com.

## Using Eggplant DAI GitHub Action in your workflow

In order to use the Eggplant DAI GitHub Action, you need to add this to your GitHub Workflow .yml file:

```yaml
name: "YOUR WORK FLOW NAME"

# Configure which branch that will trigger Eggplant DAI GitHub Action
# on:
#  push:
#    branches:
#      - main 
#  pull_request:
#    branches:
#      - main

jobs:
  Run-DAI-Test-Configuration:
    strategy: # Optional configuration by using matrix strategy
      max-parallel: 1 # To set the maximum number of jobs that can run simultaneously 
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest] # Operating support by Eggplant DAI GitHub Action
    runs-on: ${{ matrix.os }} # Provide OS matrix
    name: Run Test Configuration
    steps:
      - run: echo "Trigger event.. ${{ github.event_name }}. Runner OS.. ${{ runner.os }}."
      - name: Run DAI Test Configugration
        uses: TestPlant/eggplant-github-action@main
        with:
          serverURL: "" # Required. Details below
          testConfigID: "" # Required. Details below
          clientSecret: "" # Required. Details below
```

## Inputs

### `serverURL`
**Required** The URL of the DAI server, e.g. `http://localhost:8000`.

### `testConfigID`
**Required** The ID of the test configuration that you want to run, e.g. `09c48b7d-fc5b-481d-af80-fcffad5d9587`.

### `clientSecret`
**Required** The client secret to use to authenticate with the DAI server, e.g. `e9c15662-8c1b-472e-930d-aa0b11726093`.<br />
             Alternatively, you could set a repo secret in `Repo Settings > Secrets > Actions` and refer to it like below:<br />
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

## Notes

1. This workflow .yml file needs to in the `.github/workflows` directory in your repository on GitHub.<br />
Reading: https://docs.github.com/en/actions/quickstart.


2. On `strategy: max-parallel: 1`: SUT(System Under Test) is locked for one test config run at a time.<br />
Hence, we can only do unilateral testing.


3. Eggplant Runner supports these OS: Linux, Windows, MacOS.
