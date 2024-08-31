# Publish NuGet to GitHub Packages Workflow

This workflow is designed to automate the process of publishing a .NET project to GitHub Packages. It builds the 
project, creates a NuGet package, and publishes it to GitHub Packages.

## Workflow File

The workflow file is located at `.github/workflows/template-publish-to-github-packages-dotnet.yml`.

## Parameters

### Inputs

- `projectPath` (string, required): Path to the project directory.
- `projectFile` (string, required): Name of the project file.
- `dotnetVersion` (string, required): Version of .NET to use.

### Secrets

- `githubToken` (string, required): GitHub token to use for the workflow.

## Usage

To use this workflow in your repository, you need to call it from another workflow file. Below are examples of how 
to use it.

### Example 1: Basic Usage

```yaml
name: Publish to GitHub Packages

on:
  push:
    branches:
      - main

jobs:
  call-publish-workflow:
    uses: brenordv/github-workflows/.github/workflows/template-publish-to-github-packages-dotnet.yml@v1
    with:
      projectPath: 'src/MyProject'
      projectFile: 'MyProject.csproj'
      dotnetVersion: '6.0.x'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
```

### Example 2: Using a Different Branch

```yaml
name: Publish to GitHub Packages

on:
  push:
    branches:
      - release

jobs:
  call-publish-workflow:
    uses: brenordv/github-workflows/.github/workflows/template-publish-to-github-packages-dotnet.yml@v1
    with:
      projectPath: 'src/AnotherProject'
      projectFile: 'AnotherProject.csproj'
      dotnetVersion: '7.0.x'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
```

## Steps

1. **Checkout code**: Uses the `actions/checkout@v4` action to checkout the repository.
2. **Set up .NET**: Uses the `actions/setup-dotnet@v4` action to set up the specified .NET version.
3. **Build the project**: Runs the `dotnet build` command to build the project.
4. **Create the NuGet package**: Runs the `dotnet pack` command to create the NuGet package.
5. **Publish to GitHub Packages**: Adds the GitHub Packages source and pushes the NuGet package using the 
`dotnet nuget push` command.

## Notes

- Ensure that the `githubToken` secret is added to your repository settings.
- Adjust the `projectPath`, `projectFile`, and `dotnetVersion` parameters according to your project structure and 
requirements.