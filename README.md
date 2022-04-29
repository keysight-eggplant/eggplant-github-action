<img src="https://www.eggplantsoftware.com/hubfs/Branding/Keysight-Eggplant-Logo_RGB_full-color.svg" width="300px"/>

# Eggplant DAI Runner 

## Introduction

The Eggplant DAI Runner is an [Eggplant DAI](https://www.eggplantsoftware.com/digital-automation-intelligence) integration tool that build as GitHub Action. It enables the functionality to launch DAI tests from within a GitHub workflow pipeline. You can use it to continuously test your application's [model-based approach to testing](https://docs.eggplantsoftware.com/docs/dai-using-eggplant-dai/).  For more information about Eggplant, visit https://www.eggplantsoftware.com.

## Using Eggplant DAI Runner in your workflow

**Step 1**: Search for **Eggplant DAI Runner** in [GitHub Marketplace](https://github.com/marketplace?category=&query=&type=actions&verification=)

![image](https://user-images.githubusercontent.com/101400930/165936757-deabc57d-f399-4134-a7b0-5bfe5e8a278c.png)

**Step 2**: Click on **Use latest version**

![image](https://user-images.githubusercontent.com/101400930/165936996-07f6517e-22c5-4edb-8dda-751a1d4809cf.png)

**Step 3**: Copy and paste the following snippet into your .yml file. 

![image](https://user-images.githubusercontent.com/101400930/165937331-3b72ff52-0444-4e97-ad7f-838888f4eabc.png)

## Sample work flow YML content
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
      - name: Eggplant DAI Runner
        uses: TestPlant/eggplant-github-action@main
        with:
          serverURL: "" # Required. Details below
          testConfigID: "" # Required. Details below
          clientSecret: "" # Required. Details below
```

## Inputs

### `serverURL`
**Required** The URL of the Eggplant DAI server, e.g. `http://localhost:8000`.

### `testConfigID`
**Required** The ID of the Eggplant DAI test configuration that you want to run, e.g. `09c48b7d-fc5b-481d-af80-fcffad5d9587`.

### `clientSecret`
**Required** The client secret to use to authenticate with the Eggplant DAI server, e.g. `e9c15662-8c1b-472e-930d-aa0b11726093`.<br />
             Alternatively, you could set a repo secret in `Repo Settings > Secrets > Actions` and refer to it like below:<br />
             `clientSecret: "${{ secrets.DAI_CLIENT_SECRET }}"`.
             
### `clientID`
**Optional** The client ID to use to authenticate with the Eggplant DAI server.<br />
**Default:** `client:dai:agent:integration`

### `requestTimeout`
**Optional** The timeout in seconds for each HTTP request to the Eggplant DAI server<br />
**Default:** `30`

### `requestRetries`
**Optional** The number of times to attempt each HTTP request to the Eggplant DAI server<br />
**Default:** `5`

### `backoffFactor`
**Optional** The exponential backoff factor between each HTTP request<br />
**Default:** `0.5`

### `pollInterval`
**Optional** The number of seconds to wait between each call to the Eggplant DAI server<br />
**Default:** `5`

### `logLevel`
**Optional** The logging level<br />
**Default:** `INFO`

### `CACertPath`
**Optional** The path to an alternative Certificate Authority pem file<br />

## Output
### Pipeline triggered
Based on the pipeline .yml configuration, when there is commits or pull request action performed. The pipeline will be triggered and Eggplant DAI Runner will be executed.

![image](https://user-images.githubusercontent.com/101400930/165939235-3f1f5ecd-8242-450d-918e-dbeb9f6f4b15.png)

### Console output
![image](https://user-images.githubusercontent.com/101400930/165939424-d096cb17-b93a-428d-a36f-a2174eef2215.png)



## Notes

1. This workflow .yml file needs to in the `.github/workflows` directory in your repository on GitHub.<br />
Reading: https://docs.github.com/en/actions/quickstart.


2. On `strategy: max-parallel: 1`: SUT(System Under Test) is locked for one Eggplant DAI test configuration run at a time.<br />
Hence, we can only do unilateral testing.


3. Eggplant DAI Runner supports 3 type of operating system: 
 - Linux
 - Windows
 - MacOS

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
