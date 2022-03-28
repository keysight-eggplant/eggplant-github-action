[![Eggplant Runner Build](https://github.com/TestPlant/eggplant-github-action/actions/workflows/eggplant-runner-demo.yml/badge.svg)](https://github.com/TestPlant/eggplant-github-action/actions/workflows/eggplant-runner-demo.yml)

# Eggplant Runner in CI/CD with GitHub Actions

1. Eggplant Runner currently provides "Run Test Config" as its main action.
2. In order to use the Eggplant Runner with GitHub Actions, you need to add this to your GitHub Workflow:
(Notes at the bottom)

```yaml
name: Eggplant Runner Build

# Configure the trigger of your workflow
on:
  pull_request:
    branches:
      - main
  push:
    branches:
      - main

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
          dryRun: true
          serverURL: "http://localhost:8000"
          testConfigID: "09c48b7d-fc5b-481d-af80-fcffad5d9587"
          clientSecret: "e9c15662-8c1b-472e-930d-aa0b11726093"
```

# Notes

1. This workflow .yml file needs to in the `.github/workflows` directory in your repository on GitHub. More details at: https://docs.github.com/en/actions/quickstart.


2. On `strategy: max-parallel: 1`: SUT(System Under Test) is locked for one test config run at a time. Hence, we can only do unilateral testing.


3. Eggplant Runner supports these OS: Linux, Windows, MacOS.


4. The parameters used in the above workflow .yml example:
  - `dryRun`: Dry Run mode validates the parameters without requiring a connection to a DAI server. Default: `False`.
  - `serverURL`: The URL of the DAI server, e.g. `http://localhost:8000`.
  - `testConfigID`: The ID of the test config that you want to run, e.g. `09c48b7d-fc5b-481d-af80-fcffad5d9587`.
  - `clientSecret`: The client secret to use to authenticate with the DAI server, e.g. `e9c15662-8c1b-472e-930d-aa0b11726093`.<br />
                    Alternatively, one could set a repo secret in `GitHub Settings > Secrets > Actions` and refer to it like below:<br />
                    `clientSecret: "${{ secrets.DAI_CLIENT_SECRET }}"`.
  - Details of more parameters including their description and default values are in `actions/run-test-config/action.yml`.
