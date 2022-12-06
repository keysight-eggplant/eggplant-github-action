<img src="https://www.eggplantsoftware.com/hubfs/Branding/Keysight-Eggplant-Logo_RGB_full-color.svg" width="300px"/>

# Eggplant DAI Runner 

## Introduction

The Eggplant DAI Runner is an [Eggplant DAI](https://www.eggplantsoftware.com/digital-automation-intelligence) integration tool that build as GitHub Action. It enables the functionality to launch DAI tests from within a GitHub workflow pipeline. You can use it to continuously test your application's [model-based approach to testing](https://docs.eggplantsoftware.com/docs/dai-using-eggplant-dai/).  For more information about Eggplant, visit https://www.eggplantsoftware.com.

The core integration of the **Eggplant DAI Runner** are with [**DAI Test Configuration**](https://docs.eggplantsoftware.com/docs/dai-test-configuration/). **Eggplant DAI Runner** basically will communicate with the API services provided by **Eggplant DAI** to perform test configuration execution.

## Using Eggplant DAI Runner in your workflow

**Step 1**: Search for **Eggplant DAI Runner** in [GitHub Marketplace](https://github.com/marketplace?category=&query=&type=actions&verification=)

![image](https://user-images.githubusercontent.com/101400930/174242174-8aa9fba1-52e2-4016-a8a0-4d7998d07f6d.png)

**Step 2**: Click on **Use latest version**
![image](https://user-images.githubusercontent.com/101400930/168304958-ed1e07b9-6738-42f8-a2e4-e8fa761daedb.png)

**Step 3**: Copy and paste the following snippet into your .yml file. 

![image](https://user-images.githubusercontent.com/101400930/174241940-fbefd241-12e9-4f03-b6c4-d8547396e80a.png)

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
      - name: Eggplant Runner
        uses: keysight-eggplant/eggplant-github-action@1.0.0
        with:
          serverURL: "" # Required. Details below
          testConfigID: "" # Required. Details below
          clientSecret: "" # Required. Details below
```

## Inputs

### `serverURL`
**Required** The URL of the Eggplant DAI server, `http(s)://dai_server_hostname:portnumber`.

### `testConfigID`
**Required** The ID of the Eggplant DAI test configuration that you want to run, e.g. `09c48b7d-fc5b-481d-af80-fcffad5d9587`.
Test configuration ID can be obtain by go to test config > look for a particular test config > test config id can be obtain from url.
![image](https://user-images.githubusercontent.com/103989779/199636740-57d4bfd2-3c94-449c-b2d5-597d69d2f03e.png)


### `clientSecret`
**Required** The client secret to use to authenticate with the Eggplant DAI server, e.g. `e9c15662-8c1b-472e-930d-aa0b11726093`.<br />
             Alternatively, you could set a repo secret in `Repo Settings > Secrets > Actions` and refer to it like below:<br />
             `clientSecret: "${{ secrets.DAI_CLIENT_SECRET }}"`.
The **DAI Client Secret** can be obtain by go to http://kc-dai_server_hostname:portnumber/auth > clients > search for client:dai:agent:integration > credential > secret
![image](https://user-images.githubusercontent.com/103989779/199636556-700ac626-87ad-4373-ab97-b78773f25176.png)

             
### `clientID`
**Required** The client ID to use to authenticate with the Eggplant DAI server.<br />
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

### `testEnvironmentTimeout`
**Optional** The timeout in seconds for checking test environment readiness<br />
**Default:** `15`

### `logLevel`
**Optional** The logging level<br />
**Default:** `INFO`

### `CACertPath`
**Optional** The path to an alternative Certificate Authority pem file<br />

### `testResultPath`
**Optional** Path to a file where the test results will be stored in junit xml format<br />
**Example** `C:\results\result.xml`

### `eggplantRunnerPath`
**Optional** The path to eggplant runner CLI executable<br />

## Output
### Pipeline triggered
Based on the pipeline .yml configuration, when there is commits or pull request action performed. The pipeline will be triggered and Eggplant DAI Runner will be executed.

![image](https://user-images.githubusercontent.com/101400930/165939235-3f1f5ecd-8242-450d-918e-dbeb9f6f4b15.png)

### Console output
![image](https://user-images.githubusercontent.com/103989779/199637972-94c8e9ea-8e96-40fa-8969-8daef5802348.png)

## Release for DAI
<table>
  <thead>
    <tr>
      <th width="300px">DAI Version</th>
      <th width="500px">Release</th>
    </tr>
  </thead>
  <tbody>
   <tr>
      <td>6.4.0-3</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner">latest</a></td>
  </tr>
  <tr>
      <td>6.3.0-3</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.4">v1.0.4</a></td>
  </tr>
  <tr>
      <td>6.2.1-2</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.3">v1.0.3</a> | <a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.2">v1.0.2</a></td>
  </tr>
  <tr>
      <td>6.1.2-1</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.1">v1.0.1</a></td>
  </tr>
  </tbody>
</table>

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
