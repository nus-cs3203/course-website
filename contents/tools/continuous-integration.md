<br>

<frontmatter>
  layout: default.md
  pageNav: 2
  pageNavTitle: "Chapters of This Page"
</frontmatter>

[](#continuous-integration)Continuous Integration
=================================================

CI is the practice of automating the integration of code changes from multiple contributors into a single software project. There are a few tools that you can use for continuous integration.

[](#1-warning)1\. Warning
=========================

Note that your project submission is going to be tested locally.

Please ensure that your project builds correctly on local machines, even if it builds successfully on CI tools.

[](#2-jenkins)2\. Jenkins
=========================

[Jenkins](https://www.jenkins.io/) is one of the tools that you can use for kick-starting CI directly in your repository. It provides hundreds of plugins to support building, deploying and automating any project.

You can look into how to integrate your repository with Jenkins [here](https://www.blazemeter.com/blog/how-to-integrate-your-github-repository-to-your-jenkins-project) and [here](https://www.jenkins.io/solutions/github/).

[](#3-github-actions)3\. GitHub Actions
=======================================

GitHub Actions is another tool that you can use. It makes use of workflows, which are custom automated processes that you can set up in your repository to build, test, package, release or deploy any project on GitHub. A workflow should be configured using YAML syntax, and save them as workflow files in your repository. You must store workflows in the .github/workflows directory in the root of your repository.

Please refer GitHub Docs on [GitHub Actions](https://docs.github.com/en/actions/learn-github-actions) for more information.
<!--
[](#31-github-actions-usage-guidelines)3.1. GitHub Actions Usage Guidelines
---------------------------------------------------------------------------

As there are limits for GitHub Actions usage for organizations, and there is currently a rise in the number of teams wanting to make use of GitHub Actions, the following are a few guidelines that you are encouraged to follow when using GitHub Actions in your project repository to ensure fair usage for all the teams.

1.  **Processes to be automated**: You can automate build processes and make use of a linter to check for stylistic errors. You can also automate any testing, which include unit testing, integration testing, regression testing and system testing.

2.  **Type of build**: You should only automate build processes on Release mode. _**You should not automate it on Debug mode.**_

4.  **Events that can trigger workflow**: You should configure all workflow to start after pushing commits or merging a pull request to the remote Git repository’s master branch. _**You should not configure your workflow to start after any pushes or merges to any other branches.**_

5.  **Frequency of triggering workflow**: You should only run your workflow at most twice a day on average. If you are doing triggering your workflow from pushes or merging more than twice a day on average, please execute your workflow on a schedule instead.

6.  **Time limit for workflow**: You should configure your workflow such that for each job (build, stylistic check), it should only run at most 3 minutes per job. You should also configure your workflow such that it runs at most 5 minutes for all workflow(s). If a workflow exceeds 5 minutes, consider increasing the time limit for the job, while decreasing the frequency of triggering the workflow.
-->

[](#31-sample-github-actions-workflow)3.1. Sample GitHub Actions Workflow
-------------------------------------------------------------------------

The following are sample workflows that you can use for Github Actions.

[Workflow for building on Windows CMake solution](../../github-actions-yml-files/build-cp-windows.yml)

[Workflow for building on MacOS](../../github-actions-yml-files/build-cp-mac.yml)

[Workflow for building on Linux](../../github-actions-yml-files/build-cp-linux.yml)

**Notes:**

1.  Please note that the sample workflow only helps to automate build processes, but it does not consist of a linter to check for stylistic errors.
2.  The scheduling (commented) on Windows/Linux will trigger the workflow at 0900hrs and 2300hrs, and the scheduling on MacOS will trigger the workflow at 2300hrs. Please feel free to change the scheduling that suits your project team.
3.  If you are building on Linux, please also note that the sample workflow for Linux is build for Ubuntu, but not Fedora. GitHub Actions do not support Fedora at the time of writing. Please use it with discretion.
4.  If you are building on Linux for GitHub Actions, you should also add the line at line 4 of Team00/Code00/src/autotester/CMakeLists.txt: target\_link\_options(autotester PUBLIC "-no-pie")
