# Raccoon Ninja's Github Workflow repository
I'm centralizing here the workflows that I use in my repositories. Feel free to use them in your projects.

# Workflows
## [Dotnet] SonarCloud Analysis
This workflow performs a SonarCloud analysis on your .NET project. It includes steps to install necessary 
tools, set up the environment, and run the analysis.

**Inputs:**
- `projectKey`: SonarCloud project key (required)
- `organization`: SonarCloud organization (required)
- `branchName`: Branch to run analysis on (optional)
- `verbose`: Enable verbose logging for SonarCloud analysis (optional, default: true)
- `sonarExclusions`: Files and directories to exclude from SonarCloud analysis (optional)
- `coverageExclusions`: Files and directories to exclude from coverage (optional)

**Secrets:**
- `githubToken`: GitHub token for PR information and checkouts (required)
- `sonarToken`: SonarCloud token for authentication (required)

### Example usage
```yaml
name: SonarCloud analysis on pull requests

on:
  push:
    branches:
      - '**'
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  sonarcloud-analysis:
    uses: brenordv/github-workflows/.github/workflows/template-qa-sonarcloud.yml
    with:
      projectKey: 'brenordv_nightscout-companion-apps'
      organization: 'raccoon-ninja'
      verbose: true
      sonarExclusions: '**/*Usings.cs,Raccoon.Ninja.TestHelpers/*'
      coverageExclusions: '**/*Usings.cs,Raccoon.Ninja.TestHelpers/*,**/*Program.cs,Raccoon.Ninja.WForm.GlucoseIcon/Utils/AppSettings.cs,Raccoon.Ninja.AzFn.DataApi/*Func.cs,Raccoon.Ninja.AzFn.ScheduledTasks/*Func.cs'

    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
      sonarToken: ${{ secrets.SONAR_TOKEN }} 
```

## [Dotnet] Full Publish Pipeline Workflow
This workflow tests, builds, creates a new release with the .zip artifacts, and deploys to Azure Functions
if specified.

**Inputs:**
- `dotnetVersion`: The version of .NET to use (required)
- `targetPlatform`: The target platform for the build (required)
- `targetRuntime`: The target runtime for the build (required)
- `projectFolder`: The folder where the project is located (required)
- `projectFile`: The project file (required)
- `releaseFilePrefix`: The prefix for the release file (required)
- `publishToAzure`: If true, the workflow will deploy the new release to the Azure Functions App (required)

**Secrets:**
- `githubToken`: The GitHub token to use for the workflow (required)
- `azureFunctionAppName`: The name of the Azure Functions App to deploy to (required only if `publishToAzure` is true)
- `azurePublishProfile`: The Azure Publish Profile for deployment (required only if `publishToAzure` is true)

### Example usage
```yaml
name: Data Transfer Function CI/CD

on:
  push:
    branches:
      - master
  workflow_dispatch:

permissions:
  contents: write
  deployments: write

jobs:
  deploy-using-template:
    uses: brenordv/github-workflows/.github/workflows/template-test-build-and-publish-to-azure.yml
    with:
      dotnetVersion: '8.0.x'
      targetPlatform: win-x64
      targetRuntime: net8.0
      projectFolder: Raccoon.Ninja.AzFn.ScheduledTasks
      projectFile: Raccoon.Ninja.AzFn.ScheduledTasks.csproj
      releaseFilePrefix: "AzFnScheduledTasks"
      publishToAzure: true
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
      azureFunctionAppName: ${{ secrets.AZFN_APP_NAME }}
      azurePublishProfile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE }}
```
