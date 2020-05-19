# GitLab101

Table of Contents

1. Introduction to GIT


GitLab is the first single application for software development, security, and operations that enables Concurrent DevOps, making the software lifecycle faster and radically improving the speed of business.

GitLab provides solutions for each of the stages of the DevOps lifecycle:
![GitLabStages](https://github.com/jawad1989/GitLab101/blob/master/images/devops-stages.png)


Git Lab Flow Example:

## Example 1:

![GitLabStages](https://github.com/jawad1989/GitLab101/blob/master/images/gitlab_workflow_example_11_9.png)

## Example 2:

![GitLabStages](https://github.com/jawad1989/GitLab101/blob/master/images/gitlab_workflow_example_extended_v12_3.png)

## Continuous Integration

Consider an application that has its code stored in a Git repository in GitLab. Developers push code changes every day, multiple times a day. For every push to the repository, you can create a set of scripts to build and test your application automatically, decreasing the chance of introducing errors to your app.

This practice is known as Continuous Integration; for every change submitted to an application - even to development branches - it’s built and tested automatically and continuously, ensuring the introduced changes pass all tests, guidelines, and code compliance standards you established for your app.

GitLab itself is an example of using Continuous Integration as a software development method. For every push to the project, there’s a set of scripts the code is checked against.

## Continuous Delivery
Continuous Delivery is a step beyond Continuous Integration. Your application is not only built and tested at every code change pushed to the codebase, but, as an additional step, it’s also deployed continuously, though the deployments are triggered manually.

This method ensures the code is checked automatically but requires human intervention to manually and strategically trigger the deployment of the changes.

## Continuous Deployment
Continuous Deployment is also a further step beyond Continuous Integration, similar to Continuous Delivery. The difference is that instead of deploying your application manually, you set it to be deployed automatically. It does not require human intervention at all to have your application deployed.

## Introduction to GitLab CI/CD
GitLab CI/CD is a powerful tool built into GitLab that allows you to apply all the continuous methods (Continuous Integration, Delivery, and Deployment) to your software with no third-party application or integration needed.

For an overview, see [Introduction to GitLab CI](https://www.youtube.com/watch?v=l5705U8s_nQ&t=397) from a recent GitLab meetup
 
 
## How GitLab CI/CD works
To use GitLab CI/CD, all you need is an application codebase hosted in a Git repository, and for your build, test, and deployment scripts to be specified in a file called [.gitlab-ci.yml](https://docs.gitlab.com/ee/ci/yaml/README.html), located in the root path of your repository.

In this file, you can define the scripts you want to run, define include and cache dependencies, choose commands you want to run in sequence and those you want to run in parallel, define where you want to deploy your app, and specify whether you will want to run the scripts automatically or trigger any of them manually. Once you’re familiar with GitLab CI/CD you can add more advanced steps into the configuration file.

## GitLab Runner
GitLab Runner is the open source project that is used to run your jobs and send the results back to GitLab. It is used in conjunction with GitLab CI/CD, the open-source continuous integration service included with GitLab that coordinates the job

### installing and Configuring a Runner

```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
sudo apt-get install gitlab-runner

```
#### To register a Runner under GNU/Linux:

1. Run the following command:
```
sudo gitlab-runner register
```

2. Enter your GitLab instance URL:
```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
https://gitlab.com
```

3. Enter the token you obtained to register the Runner:
```
Please enter the gitlab-ci token for this runner
xxx
```

4. Enter a description for the Runner, you can change this later in GitLab’s UI:
```
Please enter the gitlab-ci description for this runner
[hostname] my-runner
```

5. Enter the tags associated with the Runner, you can change this later in GitLab’s UI:
```
Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag
```

6. Enter the Runner executor:
```
Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker
```

7. If you chose Docker as your executor, you’ll be asked for the default image to be used for projects that do not define one in 
```
.gitlab-ci.yml:
```

8. Please enter the Docker image (eg. ruby:2.6):
```
alpine:latest
```

## .gitlab-ci.yml
https://docs.gitlab.com/ee/ci/yaml/README.html

The .gitlab-ci.yml file is where you configure what CI does with your project. It lives in the root of your repository.

On any push to your repository, GitLab will look for the .gitlab-ci.yml file and start jobs on Runners according to the contents of the file, for that commit.

## [Getting started with GitLab CI/CD](https://docs.gitlab.com/ee/ci/quick_start/README.html)

This page guides you how to 
1. Creae a .gitlab-ci.yml file
2. install and register a runner
3. see the pipeline status

More Eamples can be seen at [GitLAB CICD](https://docs.gitlab.com/ee/ci/examples/README.html)
