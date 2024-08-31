# Full Publish Pipeline Workflow for Dotnet

This GitHub Actions workflow is designed to test, build, create a new release with the .zip artifacts, and deploy to
Azure Functions. It automates the process of checking out the code, setting the release version, running tests, 
building the project, creating a release, and optionally deploying to Azure Functions.

## Workflow Overview

The workflow consists of multiple jobs that run on Ubuntu and Windows runners. It performs the following steps:

1. **Set Release Version**: Determines the next release version based on the `AssemblyVersion` in the project file.
2. **Test**: Runs tests for the project.
3. **Check Tag**: Checks if the release tag already exists and if the release asset is present.
4. **Validate Azure Inputs for Publishing**: Validates Azure inputs if publishing to Azure is enabled.
5. **Build Info**: Logs build information.
6. **Build**: Builds the project and creates a .zip artifact.
7. **Deploy**: Deploys the artifact to Azure Functions and creates a GitHub release.

## Inputs

The workflow accepts the following inputs:

- **dotnetVersion**: The version of .NET to use. This input is required.
- **targetPlatform**: The target platform for the build. This input is required.
- **targetRuntime**: The target runtime for the build. This input is required.
- **projectFolder**: The folder where the project is located. This input is required.
- **projectFile**: The project file. This input is required.
- **releaseFilePrefix**: The prefix for the release file. This input is required.
- **publishToAzure**: If true, the workflow will deploy the new release to the Azure Functions App. This input is required.

## Secrets

The workflow requires the following secrets:

- **githubToken**: GitHub token to use for the workflow. This secret is required.
- **azureFunctionAppName**: The name of the Azure Functions App to deploy to. This secret is required only 
if `publishToAzure` is true.
- **azurePublishProfile**: The Azure Publish Profile for deployment. This secret is required only 
if `publishToAzure` is true.

## Example Usage

To use this workflow, you can call it from another workflow file. Below are some examples of how to use it:

### Example 1: Basic Usage

```yaml
name: Full Publish Pipeline

on: [push, pull_request]

jobs:
  full-publish:
    uses: brenordv/github-workflows/.github/workflows/template-test-build-and-publish-to-azure.yml@v1
    with:
      dotnetVersion: '8.0.x'
      targetPlatform: 'win-x64'
      targetRuntime: 'net8.0'
      projectFolder: 'src/MyProject'
      projectFile: 'MyProject.csproj'
      releaseFilePrefix: 'MyProject'
      publishToAzure: false
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
```

### Example 2: Publishing to Azure

```yaml
name: Full Publish Pipeline

on: [push, pull_request]

jobs:
  full-publish:
    uses: brenordv/github-workflows/.github/workflows/template-test-build-and-publish-to-azure.yml@v1
    with:
      dotnetVersion: '8.0.x'
      targetPlatform: 'win-x64'
      targetRuntime: 'net8.0'
      projectFolder: 'src/MyProject'
      projectFile: 'MyProject.csproj'
      releaseFilePrefix: 'MyProject'
      publishToAzure: true
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
      azureFunctionAppName: ${{ secrets.AZURE_FUNCTION_APP_NAME }}
      azurePublishProfile: ${{ secrets.AZURE_PUBLISH_PROFILE }}
```

## Notes

- Ensure that the path provided in `projectFolder` is relative to the root of the repository.
- The `dotnetVersion` input allows specifying a version range (e.g., `8.0.x`), which will use the latest available version within that range.
- The `publishToAzure` input should be set to `true` if you want to deploy the new release to the Azure Functions App.

This workflow simplifies the process of testing, building, creating releases, and deploying .NET projects to 
Azure Functions, ensuring consistency and reliability in your CI/CD pipeline.