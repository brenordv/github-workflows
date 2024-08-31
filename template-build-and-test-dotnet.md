# .NET Build and Test Workflow

This GitHub Actions workflow is designed to build and test .NET projects. It can be used to automate the process of 
checking out the code, setting up the .NET SDK, restoring dependencies, building the project, and running tests.

## Workflow Overview

The workflow consists of a single job that runs on an Ubuntu runner. It performs the following steps:

1. **Checkout**: Checks out the repository to the runner.
2. **Setup .NET**: Sets up the specified version of the .NET SDK.
3. **Restore dependencies**: Restores the project dependencies.
4. **Build**: Builds the project using the specified configuration.
5. **Test**: Runs the tests for the project.

## Inputs

The workflow accepts the following inputs:

- **projectOrSolutionPath**: The path to the project or solution file. This input is required.
- **dotnetVersion**: The version of the .NET SDK to use. This input is optional and defaults to `8.0.x`.
- **buildConfiguration**: The build configuration to use. This input is optional and defaults to `Release`.

## Example Usage

To use this workflow, you can call it from another workflow file. Below are some examples of how to use it:

### Example 1: Basic Usage

```yaml
name: Build and Test

on: [push, pull_request]

jobs:
  build-and-test:
    uses: brenordv/github-workflows/.github/workflows/template-build-and-test-dotnet.yml@v1
    with:
      projectOrSolutionPath: 'src/MyProject/MyProject.csproj'
```

### Example 2: Specifying .NET Version and Build Configuration

```yaml
name: Build and Test

on: [push, pull_request]

jobs:
  build-and-test:
    uses: brenordv/github-workflows/.github/workflows/template-build-and-test-dotnet.yml@v1
    with:
      projectOrSolutionPath: 'src/MyProject/MyProject.csproj'
      dotnetVersion: '7.0.x'
      buildConfiguration: 'Debug'
```

### Example 3: Using a Solution File

```yaml
name: Build and Test

on: [push, pull_request]

jobs:
  build-and-test:
    uses: brenordv/github-workflows/.github/workflows/template-build-and-test-dotnet.yml@v1
    with:
      projectOrSolutionPath: 'MySolution.sln'
```

## Notes

- Ensure that the path provided in `projectOrSolutionPath` is relative to the root of the repository.
- The `dotnetVersion` input allows specifying a version range (e.g., `7.0.x`), which will use the latest available 
version within that range.
- The `buildConfiguration` input can be set to any valid build configuration defined in your project 
(e.g., `Release`, `Debug`).

This workflow simplifies the process of building and testing .NET projects, ensuring consistency and reliability in 
your CI/CD pipeline.
