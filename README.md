# CI Pipeline for Java Web Application with AWS CodePipeline, CodeCommit, CodeBuild, and SonarCloud

This is a sample CI pipeline for a Java web application that uses AWS services like CodePipeline, CodeCommit, CodeBuild, and CodeArtifact for artifact storage. The pipeline is designed to build and test the application automatically whenever changes are pushed to the CodeCommit repository.

## Prerequisites

Before you begin, you'll need the following:

* An AWS account with permissions to create IAM roles, CodeArtifact repositories, CodeCommit repositories, CodeBuild projects, and CodePipeline pipelines.
* A Java web application that can be built with Maven.
* A SonarCloud account with a SonarCloud token for analyzing your code.

## Architecture

The CI/CD pipeline consists of the following:

* A CodeCommit repository that stores your Java web application code.
* A CodeBuild project that builds the application and runs JUnit tests.
* A SonarCloud project that analyzes the code and provides code quality metrics.
* An AWS CodeArtifact repository to store the build and deployment artifacts.
* An IAM role for CodeCommit to access the repository.
* An IAM role for CodeBuild to access the repository, CodeArtifact repository, and SonarCloud.
* An IAM role for CodePipeline to access the repository, CodeArtifact repository, and CodeBuild.

![Alt text](architecture.png "Architecture")

## Setup Instructions

To set up the CI pipeline, follow these steps:

1. Clone this repository to your local machine.

2. Create a new CodeCommit repository to store your Java web application code.

3. Create a new AWS CodeArtifact repository to store your build and deployment artifacts.

4. Create a new IAM role for CodeCommit to access the repository. Attach the `AWSCodeCommitReadOnly` policy to the role.

5. Create a new IAM role for CodeBuild to access the repository, CodeArtifact repository, and SonarCloud.

6. Create a new IAM role for CodePipeline to access the repository, CodeArtifact repository, and CodeBuild.

7. Create a new SonarCloud project and generate a SonarCloud token.

8. Edit the `buildspec.yml` file to include the CodeArtifact upload command for your build artifact. Replace `your-codeartifact-repository` with the name of the CodeArtifact repository you created in step 4 and `your-application-artifact.jar` with the name of your application artifact.

```
artifacts:
  files:
    - target/your-application-artifact.jar
  base-directory: 'target'
  discard-paths: yes
  name: your-application-artifact.jar
  # Upload the artifact to CodeArtifact
  - aws codeartifact push-package --repository your-codeartifact-repository --format maven --namespace com.example --package your-application-artifact --version 1.0.0 --s3-bucket your-bucket-name --s3-key your-bucket-key
```

9. Commit the changes to your CodeCommit repository.

10. Create a new CodePipeline pipeline and configure the source, build, and deployment stages:

    * **Source stage**: Select your CodeCommit repository as the source provider and the IAM role for CodeCommit as the service role.
    * **Build stage**: Select your CodeBuild project as the build provider and the IAM role for CodeBuild as the service role. Use the `buildspec.yml` file in your repository as the build specification.
    * **Test stage**: Add a new action to run JUnit tests. Use the `buildspec.yml` file in your repository as the build specification.
    * **SonarCloud stage**: Add a new action to analyze the code with SonarCloud. Use the `sonarbuildspec.yml` file in your repository as the build specification.

11. Save the pipeline configuration and run the pipeline.

## Conclusion

This CI pipeline is designed to help you automate the build and test process for your Java web application. With AWS services like CodePipeline, CodeCommit, CodeBuild, and CodeArtifact, you can easily set up a pipeline that builds and deploys your application automatically whenever changes are pushed to the repository. By using SonarCloud for code analysis, you can also get valuable insights into the quality of your code and identify areas for improvement. By storing the build artifact in an AWS CodeArtifact repository, you can ensure that the artifact is available for deployment, even if the original CodeBuild instance is no longer available.
