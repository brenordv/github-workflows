# Create GitHub Release and Upload Artifacts Workflow

This GitHub Actions workflow is designed to create a GitHub release and upload build artifacts. It automates the process
of checking out the code, setting the release version, creating a tag, creating a release, and optionally building and 
uploading NuGet packages and ZIP artifacts.

## Workflow Overview

The workflow consists of multiple jobs that run on Ubuntu, Windows, and macOS runners. It performs the following steps:

1. **Set Release Version**: Determines the next release version based on the specified tag in the project file.
2. **Create Tag**: Creates a new Git tag for the release.
3. **Create Release**: Creates a GitHub release with the specified tag.
4. **Build NuGet Artifact** (optional): Builds the project and uploads the NuGet package to the release.
5. **Build ZIP Artifacts**: Builds the project for different platforms and uploads the ZIP artifacts to the release.

## Inputs

The workflow accepts the following inputs:

- **projectPath**: The path to the project folder. This input is required.
- **projectFile**: The name of the project file. This input is required.
- **csprojTag**: The tag in the project file to fetch the version. This input is optional and defaults to `AssemblyVersion`.
- **dotnetVersion**: The version of the .NET SDK to use. This input is optional and defaults to `8.0.x`.
- **releaseFilePrefix**: The prefix for the release artifact. This input is optional with no default.
- **isNugetPackage**: Indicates if the build produces a NuGet package. This input is optional and defaults to `false`.

## Secrets

The workflow requires the following secrets:

- **githubToken**: GitHub token to use for the workflow. This secret is required.

## Example Usage

To use this workflow, you can call it from another workflow file. Below are some examples of how to use it:

### Example 1: Basic Usage

```yaml
name: Create Release and Upload Artifacts

on: [push, pull_request]

jobs:
  create-release:
    uses: brenordv/github-workflows/.github/workflows/template-create-release-and-artifacts.yml@v1
    with:
      projectPath: 'src/MyProject'
      projectFile: 'MyProject.csproj'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
```

### Example 2: Specifying .NET Version and Release File Prefix

```yaml
name: Create Release and Upload Artifacts

on: [push, pull_request]

jobs:
  create-release:
    uses: brenordv/github-workflows/.github/workflows/template-create-release-and-artifacts.yml@v1
    with:
      projectPath: 'src/MyProject'
      projectFile: 'MyProject.csproj'
      dotnetVersion: '7.0.x'
      releaseFilePrefix: 'MyProject'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
```

### Example 3: Building a NuGet Package

```yaml
name: Create Release and Upload Artifacts

on: [push, pull_request]

jobs:
  create-release:
    uses: brenordv/github-workflows/.github/workflows/template-create-release-and-artifacts.yml@v1
    with:
      projectPath: 'src/MyProject'
      projectFile: 'MyProject.csproj'
      isNugetPackage: true
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
```

## Notes

- Ensure that the path provided in `projectPath` is relative to the root of the repository.
- The `dotnetVersion` input allows specifying a version range (e.g., `7.0.x`), which will use the latest available 
version within that range.
- The `releaseFilePrefix` input can be set to any valid prefix for the release artifacts.
- The `isNugetPackage` input should be set to `true` if the build produces a NuGet package.

This workflow simplifies the process of creating releases and uploading artifacts, ensuring consistency and reliability
in your CI/CD pipeline.