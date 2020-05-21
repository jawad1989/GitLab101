Table of Contents

1. [Introduction to GIT](#gitlab101)
2. [Continuous Integration](#continuous-integration)
3. [Continuous Delivery](#continuous-delivery)
4. [Continuous Deployment](#continuous-deployment)
5. [Introduction to GitLab CI/CD](#introduction-to-gitlab-cicd)
6. [GitLab Runner](#gitlab-runner)
   * [Installing and Configuration](#installing-and-configuring-a-runner)
7. [What is GITLAB-CI.yml](#gitlab-ciyml)
8. [Part1: Creating Your First CICD Pipeline](#create-your-first-ci-cd-pipeline)
9. [Part2: Create a CI Pipeline for Go Lang](#part-2-create-a-ci-pipeline-for-go-lang)
10. [Part3: Adding CD](#part-3-add-cd)
11. [Running Jobs in Parallel](#running-jobs-in-parallel)
12. [Running Static Website in GitLab](#running-static-website-in-gitlab)
13. [Running Java Code in GitLab](#running-java-code-in-gitlab)
14. [Deploying Node.js App to Server](#deploye-nodejs-app-to-server-virtualbox)
15. [SSH Connection: Deploying to EC2]
 

# 1. GitLab101

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

# Create your first CI CD pipeline
 fork the project https://gitlab.com/dnsmichi/meetup-2020-cw13
 
1. Create a Project
2. pen the Web IDE (repository view, centered button)
3. Create a new file `.config-ci.yml"
 * Use Docker: “image: centos:7”
 * Define a simple job called “test”, executing a “script”
 * Commit & inspect the CI job
 *
  ```
  image: centos:7

test:
    script:
        - echo "Hello from Gitlab" && exit 1
```
4. Commit the file, create a new branch, select no merge
5. on right side click pipleine, click the pipleine and you will see pipleine getting failed.
6. Change “exit 1” to “exit 0”, commit and inspect again  

  ![pipelineCI](https://github.com/jawad1989/GitLab101/blob/master/images/exit%200.PNG)

7. Commit the file, this time pipleline will be successful.

![pipelineCI](https://github.com/jawad1989/GitLab101/blob/master/images/pipeline%20success.PNG)

![pipelineCI](https://github.com/jawad1989/GitLab101/blob/master/images/console%20log.PNG)

# Part 2: Create a CI pipeline for Go Lang 
1. create a `main.go` file
 ```
   package main

import(
    "fmt"
)

func main(){
    fmt.Println("hello from Git Lab Project")
}
```

2. goto you `.config-ci.yml` file and this time from templates choose go lang:
![GoLang Template](https://github.com/jawad1989/GitLab101/blob/master/images/go%20template.PNG)

3. Edit “REPO_NAME” with the URL to the current project (no leading https://)

![Update Template](https://github.com/jawad1989/GitLab101/blob/master/images/update%20yml.PNG)

4. Press commit, add comments, dont merge
5. You will see the pipeline running
  *  The repository provides “.gitlab-ci.solution.yml” to verify your changes.
  
6. Select pipeline, you will see the stages as defined in CI config file.
![pipeline success](https://github.com/jawad1989/GitLab101/blob/master/images/pipeline%20sucess%20-%20go%20lang.PNG)
![CICD success](https://github.com/jawad1989/GitLab101/blob/master/images/CICD%20success.PNG)

# Part 3: Add CD 
1. Open the .gitlab-ci.yml file in the Web IDE
  * Add a new stage called “deploy” in the “stages” array (if not existing)
   * Add a new job called “run”
     * stage: deploy
     * Script: ./tanuki
```
run:
  stage: deploy 
  script:
    - ./tanuki
```
2. Commit the changes
3. Inspect the CI pipeline and open the log window for the “run” job 
 ![Run complete](https://github.com/jawad1989/GitLab101/blob/master/images/run%20complet.PNG)
 
4. Monitor the Analytics
![Run complete](https://github.com/jawad1989/GitLab101/blob/master/images/analytics.PNG)

# Running jobs in parallel
https://gitlab.com/jawadxiv/portal_cfg_lw

`.config-ci.yml `
```
image: busybox:latest

before_script:
  - echo "Before script section"
  - echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"

after_script:
  - echo "After script section"
  - echo "For example you might do some cleanup here"

build1:
  stage: build
  script:
    - echo "Do your build here"

test1:
  stage: test
  script:
    - echo "Do a test here"
    - echo "For example run a test suite"

test2:
  stage: test
  script:
    - echo "Do another parallel test here"
    - echo "For example run a lint test"


test3:
  stage: test
  script:
    - echo " im the third test"

job deploy-to-production:
  stage: deploy
  script:
    - echo "Do your deploy here"

```

![Multiple Jobs](https://github.com/jawad1989/GitLab101/blob/master/images/multiple%20jobs.PNG)

# Running Static Website in GitLab
https://gitlab.com/jawadxiv/staticsite/pages

`.config-ci.yml`
```
# Full project: https://gitlab.com/pages/plain-html
pages:
  stage: deploy
  script:
    - mkdir .public
    - cp -r * .public
    - mv .public public
  artifacts:
    paths:
      - public
  only:
    - master
```

# Running Java Code in Gitlab
https://www.youtube.com/watch?v=ypbGz7kE-Bo

# Deploye Node.js app to server (VirtualBox)
https://www.youtube.com/watch?v=iQbENcbPtDo

# SSH Connection
1. Create a AWS EC2 instance 
  ![ec2](https://github.com/jawad1989/GitLab101/blob/master/images/1%20Ec2%20instance.PNG)
  
2. Add Inbound Rule to Security Group
   ![inboundRule](https://github.com/jawad1989/GitLab101/blob/master/images/2%20Security%20Groups.PNG)
2. Install Apache LAMP Stack
   ```
   sudo yum update -y
   sudo yum install -y httpd24 php72 mysql57-server php72-mysqlnd
   sudo service httpd start
   sudo chkconfig httpd on
   chkconfig --list httpd
   ```
3. Verify Installation
    ![ec2](https://github.com/jawad1989/GitLab101/blob/master/images/3%20Apache%20installed.PNG)
4. Give rights to EC2-USER
  ```
  sudo chown -R ec2-user:ec2-user /var/www/html
  ```
5. Gitlab: make a variable for EC2 host and private key
  ![variables](https://github.com/jawad1989/GitLab101/blob/master/images/4%20Variables.PNG)
6. config ci file for GIT
  ```
  stages:
    - build 
    - deploy

buildJob:
    stage: build
    image: alpine
    before_script:
        - apk add zip
    script:
        - mkdir public
        - touch public/index.html
        - echo "<h1> New Deployment </h1>" >> public/index.html
        - zip -r public.zip public
    artifacts:
        paths:
            - public.zip
        
deployJob:
    stage: deploy
    image: alpine
    before_script:
        - apk add openssh-client
        - eval $(ssh-agent -s)
        - echo "$STAGE_SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
        - mkdir -p ~/.ssh
        - chmod 700 ~/.ssh
    script:
        - scp -o StrictHostKeyChecking=no public.zip ec2-user@ec2-54-157-162-21.compute-1.amazonaws.com:/var/www/html
        - ssh -o StrictHostKeyChecking=no ec2-user@ec2-54-157-162-21.compute-1.amazonaws.com "cd /var/www/html; touch foo.txt; unzip public.zip"

  ```
7. Running Pipeline

  ![Pipeline](https://github.com/jawad1989/GitLab101/blob/master/images/5%20pipeline.PNG)
  
8. Vertification

  ![sucess](https://github.com/jawad1989/GitLab101/blob/master/images/6%20succes.PNG)
  
## GitLab Registery 
### Useful Resources
More Eamples can be seen at<br/> [GitLAB CICD](https://docs.gitlab.com/ee/ci/examples/README.html)<br/>
https://about.gitlab.com/blog/2018/01/22/a-beginners-guide-to-continuous-integration/<br/>
https://gitlab.com/gitlab-examples<br/>
Self-Practice: Try JUnit with various test frameworks: https://docs.gitlab.com/ee/ci/junit_test_reports.html <br/>
https://www.youtube.com/watch?v=l5705U8s_nQ&t=397
