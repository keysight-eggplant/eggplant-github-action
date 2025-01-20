<img src="https://www.eggplantsoftware.com/hubfs/Branding/Keysight-Eggplant-Logo_RGB_full-color.svg" width="300px"/>

# Eggplant DAI Runner

## Introduction

The Eggplant DAI Runner for Github is an [Eggplant DAI](https://www.eggplantsoftware.com/digital-automation-intelligence) integration tool that is built as or builds as a GitHub Action. It enables the functionality to launch DAI tests from within a GitHub workflow pipeline. You can use it to continuously test your application's [model-based tests](https://docs.eggplantsoftware.com/docs/dai-using-eggplant-dai/).  For more information about Eggplant, visit https://www.eggplantsoftware.com.

The core integration of the **Eggplant DAI Runner** are [**DAI test configurations**](https://docs.eggplantsoftware.com/docs/dai-test-configuration/). The **Eggplant DAI Runner** communicates with the API services provided by **Eggplant DAI** to perform test configuration execution.

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
        uses: keysight-eggplant/eggplant-github-action@v1.0.0
        with:
          serverURL: "" # Required. Details below
          testConfigID: "" # Required. Details below
          clientID: "" # Required. Details below
          clientSecret: "" # Required. Details below
```

## Inputs

### `serverURL` 
**[Required]**  The URL of the Eggplant DAI server, `http(s)://dai_server_hostname:portnumber`. 


### `testConfigID`
**[Required if testConfigName is not given]**  The ID of the Eggplant DAI test configuration that you want to run, e.g. `389fee3e-9d6b-43e6-b31e-f1d379f27cdf`. 
<br />Test configuration ID can be obtained by go to `Test Config > Look for a particular test config > Test config id can be obtain from url`.
![image](https://user-images.githubusercontent.com/103989779/199636740-57d4bfd2-3c94-449c-b2d5-597d69d2f03e.png)
<br />
Alternatively, use [testConfigName](#testconfigname) and remove this input.

### `testConfigName`
**[Required if testConfigID is not given]** The name of the Eggplant DAI test configuration that you want to run. 
<br />Must provide ***one*** of the following supporting arguments:

- ### `modelName`
DAI model name for the specified test configuration. (Use this argument if only testConfigName is provided)

- ### `suiteName`
DAI suite name for the specified test configuration. (Use this argument if only testConfigName is provided)
             
### `clientID`
**[Required]** The client ID to use to authenticate with the Eggplant DAI server. 

### `clientSecret`
 **[Required]** The client secret to use to authenticate with the Eggplant DAI server. <br />
Alternatively, you could set a repo secret in `Repo Settings > Secrets > Actions` and refer to it like below:<br />
`clientSecret: "${{ secrets.DAI_CLIENT_SECRET }}"`.

The **DAI Client Secret** can be obtained by go to `http(s):/dai_server_hostname:portnumber/ > System > API Access > Add New` (for new API access creation)

![image](https://user-images.githubusercontent.com/101400930/206938890-07a45761-3c49-40a7-bf48-1a1b6f3b3659.png)

### `requestTimeout`
 **[Optional]** The timeout in seconds for each HTTP request to the Eggplant DAI server.<br />
**Default:** `30`

### `requestRetries`
**[Optional]** The number of times to attempt each HTTP request to the Eggplant DAI server.<br />
**Default:** `5`

### `backoffFactor`
**[Optional]** The exponential backoff factor between each HTTP request.<br />
**Default:** `0.5`

### `logLevel`
**[Optional]** The logging level. <br />
**Default:** `INFO`

### `CACertPath`
**[Optional]** The path to an alternative Certificate Authority pem file. <br />

### `testResultPath`
**[Optional]** The path to a file where the test results will be stored in JUnit XML format. <br />
**Example:** `C:\results\result.xml`

### `eggplantRunnerPath`
**[Optional]** The path to Eggplant runner CLI executable. <br />

### `parameters`
**[Optional]** The global parameter(s) to override in the format `parameter_name=parameter_value`. </br>
**Example:** `username=Lily` </br>
You can override multiple parameters by separating them with a delimiter of two semi-colons (`;;`).</br>
**Example:** `username=Lily;;city=Paris;;hobby=Jogging`

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
      <td>25.1.0+0</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner">latest</a></td>
   </tr>
   <tr>
      <td>7.5.0-10</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.15">v1.0.15</a></td>
   </tr>
   <tr>
      <td>7.5.0-9</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.14">v1.0.14</a></td>
   </tr>
   <tr>
      <td>7.4.0-4</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.12">v1.0.12</a></td>
   </tr>
   <tr>
      <td>7.3.0-3</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.11">v1.0.11</a></td>
   </tr>
   <tr>
      <td>7.2.0-4</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.10">v1.0.10</a></td>
   </tr>
   <tr>
      <td>7.1.0-5</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.9">v1.0.9</a></td>
   </tr>
   <tr>
      <td>7.0.1-1</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.8">v1.0.8</a></td>
   </tr>
   <tr>
      <td>7.0.0-3</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.7">v1.0.7</a></td>
   </tr>
   <tr>
      <td>6.5.0-3</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.6">v1.0.6</a></td>
   </tr>   
   <tr>
      <td>6.4.0-5</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.5">v1.0.5</a></td>
  </tr>
  <tr>
      <td>6.3.0-3</td>
      <td><a href="https://github.com/marketplace/actions/eggplant-runner?version=v1.0.4">v1.0.4</a></td>
  </tr>
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


4. Starting from v1.0.12 (DAI 7.4.0-4) onwards, Inputs `pollInterval` and `testEnvironmentTimeout` were removed. Warnings are expected if inputs are still in the workflow file.

5. If the inputs for your parameters in the workflow contain double-quote (`"`) special characters, you must escape them with three backslashes (`\\\"`).<br />
This is because double quotes (`"`) that are not escaped are used to wrap all the parameter input.<br />
Furthermore, if your parameter inputs contain a dollar sign (`$`) special character, you must escape it with two backslashes `\\$` because the dollar sign is a reserved keyword for the workflow.<br />
Example: `parameters: "value=\\\"double quote with one dollar \\$ sign\\\""`

6. Release v1.0.15 (DAI 7.5.0-10) now allows passes after re-run.

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
