# SonarCloud Analysis for Dotnet Workflow

This GitHub Actions workflow is designed to perform SonarCloud analysis on .NET projects. It automates the process of 
checking out the code, setting up the environment, and running the SonarCloud scanner to analyze the code quality and 
coverage.

## Workflow Overview

The workflow consists of a single job that runs on a Windows runner. It performs the following steps:

1. **Install Tools**: Installs necessary tools like `dotnet-coverage` and `xmldocmd`.
2. **Set up JDK**: Sets up JDK 17 required for SonarCloud analysis.
3. **Checkout Code**: Checks out the repository to the runner.
4. **Cache SonarCloud Packages and Scanner**: Caches SonarCloud packages and scanner to speed up subsequent runs.
5. **Install SonarCloud Scanner**: Installs the SonarCloud scanner if not already cached.
6. **Build and Analyze**: Builds the project and runs the SonarCloud analysis.

## Inputs

The workflow accepts the following inputs:

- **projectKey**: The SonarCloud project key. This input is required.
- **organization**: The SonarCloud organization. This input is required.
- **branchName**: The branch to run analysis on. This input is optional and defaults to the current branch.
- **verbose**: Enable verbose logging for SonarCloud analysis. This input is optional and defaults to `true`.
- **sonarExclusions**: Files and directories to exclude from SonarCloud analysis. This input is optional and defaults 
to an empty string.
- **coverageExclusions**: Files and directories to exclude from coverage. This input is optional and defaults to an 
empty string.
- **projectOrSolution**: Path to the project or solution to build and analyze. This input is optional.
- **scanAll**: Enable full scan with SonarCloud scanner. This input is optional and defaults to `false`.

## Secrets

The workflow requires the following secrets:

- **githubToken**: GitHub token for PR information and checkouts. This secret is required.
- **sonarToken**: SonarCloud token for authentication. This secret is required.

## Example Usage

To use this workflow, you can call it from another workflow file. Below are some examples of how to use it:

### Example 1: Basic Usage

```yaml
name: SonarCloud Analysis

on: [push, pull_request]

jobs:
  sonarcloud-analysis:
    uses: brenordv/github-workflows/.github/workflows/template-qa-sonarcloud.yml@v1
    with:
      projectKey: 'my-sonarcloud-project-key'
      organization: 'my-sonarcloud-organization'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
      sonarToken: ${{ secrets.SONAR_TOKEN }}
```

### Example 2: Specifying Branch and Exclusions

```yaml
name: SonarCloud Analysis

on: [push, pull_request]

jobs:
  sonarcloud-analysis:
    uses: brenordv/github-workflows/.github/workflows/template-qa-sonarcloud.yml@v1
    with:
      projectKey: 'my-sonarcloud-project-key'
      organization: 'my-sonarcloud-organization'
      branchName: 'feature-branch'
      sonarExclusions: '**/tests/**'
      coverageExclusions: '**/tests/**'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
      sonarToken: ${{ secrets.SONAR_TOKEN }}
```

### Example 3: Full Scan and Project Path

```yaml
name: SonarCloud Analysis

on: [push, pull_request]

jobs:
  sonarcloud-analysis:
    uses: brenordv/github-workflows/.github/workflows/template-qa-sonarcloud.yml@v1
    with:
      projectKey: 'my-sonarcloud-project-key'
      organization: 'my-sonarcloud-organization'
      scanAll: true
      projectOrSolution: 'src/MyProject/MyProject.csproj'
    secrets:
      githubToken: ${{ secrets.GITHUB_TOKEN }}
      sonarToken: ${{ secrets.SONAR_TOKEN }}
```

## Notes

- Ensure that the `projectKey` and `organization` inputs match your SonarCloud project and organization.
- The `branchName` input defaults to the current branch if not specified.
- The `sonarExclusions` and `coverageExclusions` inputs allow specifying files and directories to exclude from analysis
and coverage.
- The `scanAll` input enables a full scan with the SonarCloud scanner when set to `true`.

This workflow simplifies the process of running SonarCloud analysis on .NET projects, ensuring code quality and 
coverage in your CI/CD pipeline.