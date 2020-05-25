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
15. [SSH Connection: Deploying to EC2](#ssh-connectionx)
16. [Building Docker images/Docker Registery with Gitlab CI ](#building-docker-images-and-adding-to-docker-registery)
17. [Run Unit Test on HTML using Tidy and then deploy](#run-unit-test-on-html-using-tidy-and-then-deploy)
18. [Car Assembly Pipeline](#car-assembly-pipeline)
19. [Gatsby Example CI](#)
* [Cache vs Artifacs](#)
* [Adding more enviornments]
* Adding Global Variables
* Manually Triggering jobs / Manual Deployments
* Configure Merge Branch

*********************

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
  
# Building Docker Images and Adding to Docker Registery
1. Docker File

```
FROM nginx:alpine
COPY ./public /usr/share/nginx/html
```

2. Gitlab ci File
```
stages:
    - build
    
# This file is a template, and might need editing before it works on your project.
docker-build-master:
  # Official docker image.
  image: docker:latest
  stage: build
  services:
    - docker:dind
  before_script:
    - echo $CI_BUILD_TOKEN | docker login -u "$CI_REGISTRY_USER" --password-stdin $CI_REGISTRY
  script:
    - docker build --pull -t "$CI_REGISTRY_IMAGE" .
    - docker push "$CI_REGISTRY_IMAGE"
  only:
    - master
```
3. Container Registery successful once pipeline completes
![containerRegistery](https://github.com/jawad1989/GitLab101/blob/master/images/container-registery.PNG)

# Run Unit Test on HTML using Tidy and then deploy

1. tidy.sh script

make a directory `docker/install_tidy.sh'

```
#!/bin/bash

set -e
set -u

export DEBIAN_FRONTEND=noninteractive

apt-get update && apt-get install --no-install-recommends -yy tidy
```

2. index.html
```
<html>
<body>
</body>
</html>

```

3. .gitlab-ci.yml

```
image: debian:buster

variables:
    DEPLOY_FOLDER: '/srv/test'
    
stages:
    - test 
    - deploy
    
test-code:
    stage: test
    script:
        # code quality test
        - bash docker/install_tidy.sh
        - tidy -q -errors index.html

deploy-code:
    stage: deploy
    artifacts:
        paths:
            - index.html
        expire_in: 1 month
    script:
        - cp index.html "${DEPLOY_FOLDER}"
    only:
        - master
```

4. pipeline status
test stage will fail as html is not proper

```
 Selecting previously unselected package tidy.
 Preparing to unpack .../tidy_2%3a5.6.0-10_amd64.deb ...
 Unpacking tidy (2:5.6.0-10) ...
 Setting up libtidy5deb1:amd64 (2:5.6.0-10) ...
 Setting up tidy (2:5.6.0-10) ...
 Processing triggers for libc-bin (2.28-10) ...
 $ tidy -q -errors index.html
 line 1 column 1 - Warning: missing <!DOCTYPE> declaration
 line 2 column 1 - Warning: inserting missing 'title' element
Running after_script
00:01
Uploading artifacts for failed job
00:02
 ERROR: Job failed: exit code 1
```
5. new_html.html
you can change the html or use a new html
```
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>

<h1>This is a Heading</h1>
<p>This is a paragraph.</p>

</body>
</html>
```

and in `.gitlab-ci.yml` update the new html file 'new_html.html'
```
test-code:
    stage: test
    script:
        # code quality test
        - bash docker/install_tidy.sh
        - tidy -q -errors new_html.html
```

6. pipeline status

```
Uploading artifacts for successful job
00:03
 Uploading artifacts...
 index.html: found 1 matching files                 
 Uploading artifacts to coordinator... ok            id=562873680 responseStatus=201 Created token=cnWFqn39
 Job succeeded
```
# Car Assembly Pipeline

```
stages:
  - prep
  - build
  - test

preparing the file:
  stage: prep
  script:
    - mkdir build
    - cd build
    - touch car.txt
  artifacts:
    paths:
     - build/

build the car:
  stage: build
  script:
    - cd build
    - echo "jawad" >> car.txt
    - echo "saleem" >> car.txt
    - echo "hhsc" >> car.txt
  artifacts:
     paths:
      - build/
    
author:
  stage: build
  script:
    - mkdir meta
    - cd meta
    - echo $GITLAB_USERNAME > author.txt
  artifacts:
    paths:
      - meta/
      
test the car:
  stage: test
  script:
      - test -f build/car.txt
      - cd build
      - cat car.txt
      - grep "jawad" car.txt # used for searching lines that matcha regular expressions
      - grep "saleem" car.txt
      - grep "hhsc" car.txt

```
# Build Site with Gatsby

## 1. Install gatsby on localhost

   Source: https://www.gatsbyjs.org/docs/quick-start/
    
   ### * Install the Gatsby CLI
    ```
    npm install -g gatsby-cli
    ```
   ###  * Crate a new site
   ```
   gatsby new gatsby-site
   ```
   ### * Change directories into site folder
   ```
   cd gatsby-site
   ```
   ### Start Development Server
   ```
   gatsby develop
   ```
   > Gatsby will start a hot-reloading development environment accessible by default at http://localhost:8000.
  
  ![GATSby server](https://github.com/jawad1989/GitLab101/blob/master/images/1%20-%20Gatsby%20Server.PNG)
  
  
  > Try editing the JavaScript pages in src/pages. Saved changes will live reload in the browser.
   
   ### Create a production build
   ```
   gatsby build
   ```
   
   ### push code to Gitlab Repository
   
   * Create a project on Gitlab 
   * Initiate git in installed gatsby directory
   
   ![GATSby server](https://github.com/jawad1989/GitLab101/blob/master/images/2%20-push%20site.PNG)

   ### Create .gitlab-ci.yml file in Visaual Code
   * Open visual Code and the project
   * Add a new file `gitlab-ci.yml`
     ```
     build website:
      image: node
      script:
        - npm install
        - npm install -g gatsby-cli
        - gatsby build
     ```
   * Commit and push the code from visual code
    ![CommitPush](https://github.com/jawad1989/GitLab101/blob/master/images/3%20-%20Commit%20and%20push.PNG)
    
   * pipeline will complete
   ```
   success Building static HTML for pages - 2.044s - 4/4 1.96/s
   success Generating image thumbnails - 18.859s - 6/6 0.32/s
   success onPostBuild - 0.001s
   info Done building in 34.868272024 sec
    Running after_script
    00:02
    Saving cache
    00:01
    Uploading artifacts for successful job
    00:02
   Job succeeded
   ```
  > if build fails on Gitlab CI, use the latest LTS Node.js version like this:
  > image: node:10

  * Save data in artifacts 
   We want to view the artifact once the job is completed for that add below code in your job
   ```
   artifacts:
    paths:
      - ./public
   ```
   
   ![Job Artifact](https://github.com/jawad1989/GitLab101/blob/master/images/4-%20Job%20Artifact.PNG)
   
   ### Add Test Artifact Job
   
   ```
   stages:
  - build
  - test

build website:
  stage: build
  image: node
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
  artifacts:
    paths:
      - ./public

test artifact:
  stage: test
  script:
    - grep "Gatsby" ./public/index.html
    - grep "GatsbyAsda" ./public/index.html
    
   ```
   ## Running Jobs in Parallel and in Background
   
   in this job we will install node image, install npm packages and curl the server/
   
   ```
   stages:
  - build
  - test

build website:
  stage: build
  image: node
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
  artifacts:
    paths:
      - ./public

test artifact:
  image: alpine # minimilistic image 5 mb
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html

test website http:
  image: node
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
    
   ```
   
   ## Why jobs fail
   After a command is executed it will return exit status
   
   **0 - Job success**
 
   **1-255 = Job failed: exit code 1**

## Deployment using surge.sh

Surge has been built from the ground up for native web application publishing and is committed to being the best way for Front-End Developers to put HTML5 applications into production.

1. First, ensure you have a recent version of Node.js

2. Then, install Surge using npm by running the following command:

```
npm install --global surge
```

3. Now, run `surge` from within any directory, to publish that directory onto the web.

https://surge.sh/help/getting-started-with-surge

## Managing Secrets using enviornment variables
goto Your project -> Settings - > CICD -> Variables
here you can add key and value and save them.

## Deployment using surge.sh and Gitlab CICD
in this stage we will add a `deploy` stage and add a new job `deploy to surge` which will deploy our website on a domain that can be accessed from any where.

```
image: node

stages:
  - build
  - test
  - deploy

build website:
  stage: build
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
  artifacts:
    paths:
      - ./public

test artifact:
  image: alpine # minimilistic image 5 mb
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html

test website http:
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
    
deploy to surge:
  stage: deploy
  script: 
    - npm install --global surge
    - surge --project ./public --domain jawadgitlab.surge.sh

```
## Adding Smoke test stage/Job

In this stage we added a new stage and new job.
In job we will use alpine image, install curl and use curl command to find a string in our domain

```
image: node

stages:
  - build
  - test
  - deploy
  - postDeployment

build website:
  stage: build
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
  artifacts:
    paths:
      - ./public

test artifact:
  image: alpine # minimilistic image 5 mb
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html

test website http:
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
    
deploy to surge:
  stage: deploy
  script: 
    - npm install --global surge
    - surge --project ./public --domain jawadgitlab.surge.sh

deploy smoke test:
  image: alpine
  stage: postDeployment
  script:
    - apk add --no-cache curl
    - curl "http://jawadgitlab.surge.com" | grep -q "Gatsby"

```

# Pre defined Variables

https://docs.gitlab.com/ee/ci/variables/predefined_variables.html

* In this example we used predefined varialbe `$CI_COMMIT_SHORT_SHA` which displays the current version
* we added a variable in index.html file and named it `%%VERSION%%`
* using `sed` we updated the text '%%VERSION%%' with our variable '$CI_COMMIT_SHORT_SHA'.
* After this in `postDeployment` Stage we used grep to verify if version is placed on the index.html page

```
image: node

stages:
  - build
  - test
  - deploy
  - postDeployment

build website:
  stage: build
  script:
    - echo $CI_COMMIT_SHORT_SHA
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
    - sed -i "s/%%VERSION%%/$CI_COMMIT_SHORT_SHA/" ./public/index.html
  artifacts:
    paths:
      - ./public

test artifact:
  image: alpine # minimilistic image 5 mb
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html

test website http:
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
    
deploy to surge:
  stage: deploy
  script: 
    - npm install --global surge
    - surge --project ./public --domain jawadgitlab.surge.sh

deploy smoke test:
  image: alpine
  stage: postDeployment
  script:
    - apk add --no-cache curl
    - curl "http://jawadgitlab.surge.sh" | grep -q "Gatsby"
    - curl "http://jawadgitlab.surge.sh" | grep -q "$CI_COMMIT_SHORT_SHA"
```

# Schedule a Pipeline

we can schedule pipelines by below method:

![schedule pipeline](https://github.com/jawad1989/GitLab101/blob/master/images/5%20-%20pipeline%20scehedule.PNG)

# Cache pipeline Build Time

Unline jenkins Gitlab takes more time to install the dependicies first, every job starts in a clean enviornment unline workspaces in jenkins.

Cache can speed up the build times of jobs

In our project w will enable cache and save the node dependencies in cache.

```
stages:
  - build
  - test
  - deploy
  - postDeployment

cache: # global cache 
  key: ${CI_COMMIT_REF_SLUG} # we can also define our branch name e.g. master
  paths:
    - node_modules/
```

We can also Clear Cache from Pipeline view-> `Clear Runner Cache`

![Clear Cache](https://github.com/jawad1989/GitLab101/blob/master/images/6%20-%20Clear%20Runner%20Cache.PNG)


## Cache Policy 

The default cache behaviour in Gitlab CI is to download the files at the start of the job execution (pull), and to re-upload them at the end (push). This allows any changes made by the job to be persisted for future runs (known as the pull-push cache policy).

Gitlab offers the possibility of defining how a job should work with cache by setting a policy. So if we want to skip uploading cache file, we can use the setting in the cache configuration

```
policy: pull
```

### Update CI file

```
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - node_modules/
  policy: pull
```

# Running a pipeline and Job (Cache) only when scheduled

In Gitlab CI it is possible to create jobs that are only executed when a specific condition is fulfilled. For example if we want to run a job only when the pipeline is triggered by a schedule, we can configure it with:

```
only:
    - schedules
```

The same goes the other way around. If you don't want to run a job when the pipeline is triggered by a scheduled run, simply add to the respective jobs:

```
except:
    - schedules
```

Updated Pipeline:

```
image: node

stages:
  - build
  - test
  - deploy
  - postDeployment
  - cache

cache: # global cache 
  key: ${CI_COMMIT_REF_SLUG} # we can also define our branch name e.g. master
  paths:
    - node_modules/
  policy: pull

update cache:
    stage: cache
    script:
        - npm install
    cache: 
      key: ${CI_COMMIT_REF_SLUG} 
      paths:
       - node_modules/
    only:
      - schdules # this will make the job run only when pipeline is scheduled

build website:
  stage: build
  script:
    - echo $CI_COMMIT_SHORT_SHA
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
    - sed -i "s/%%VERSION%%/$CI_COMMIT_SHORT_SHA/" ./public/index.html
  artifacts:
    paths:
      - ./public
  except:
    - schdules

test artifact:
  image: alpine # minimilistic image 5 mb
  cache: {} # diable cache
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html
  except:
    - schdules

test website http:
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
  except:
    - schdules
    
deploy to surge:
  stage: deploy
  cache: {} # diable cache
  script: 
    - npm install --global surge
    - surge --project ./public --domain jawadgitlab.surge.sh
  except:
    - schdules

deploy smoke test:
  image: alpine
  cache: {} # diable cache
  stage: postDeployment
  script:
    - apk add --no-cache curl
    - curl "http://jawadgitlab.surge.sh" | grep -q "Gatsby"
    - curl "http://jawadgitlab.surge.sh" | grep -q "$CI_COMMIT_SHORT_SHA"
  except:
    - schdules


```

Create a new schedule and run the scheduled pipeline

![Scheduled](https://github.com/jawad1989/GitLab101/blob/master/images/7%20-%20Schedule%20Cache.png)

![ScheduledJob](https://github.com/jawad1989/GitLab101/blob/master/images/8%20-%20See%20Jobs.png)


# Cache vs Artifacs

**Artifact:**

* usually the out put of a build tool
* can be used to pass data btw job/stages
* designed to save compiled/generated part of build

> e.g. we used `car.txt` in above examples


**Cache:**

* are not to be used to store build results
* should only be used as a temporary storage for project dependencies


# Enviornments:

How to add a new enviornment (Production/Stage)
* Enviornments allow you to control the CD/Deployment process
* Easily track deployments
* you will know exactly what was deployed on which enviornment
* you will have full history of your deployments

In our gitlab CI file we can add environment fields in `staging` and `production` job, once completed it will create these environmeents in GITLAB CI which can be access via our `Menu->Operations->Environments`

```
environment:
    name: staging
    url: http://jawadstaging.surge.sh

environment:
    name: production
    url: http://jawadproduction.surge.sh

```

Compelte gitlab CI file:

```
image: node

stages:
  - build
  - test
  - deploy staging
  - test staging
  - deploy production
  - test production
  - cache

cache: # global cache 
  key: ${CI_COMMIT_REF_SLUG} # we can also define our branch name e.g. master
  paths:
    - node_modules/
  policy: pull

update cache:
    stage: cache
    script:
        - npm install
    cache: 
      key: ${CI_COMMIT_REF_SLUG} 
      paths:
       - node_modules/
    only:
      - schedules # this will make the job run only when pipeline is scheduled

build website:
  stage: build
  script:
    - echo $CI_COMMIT_SHORT_SHA
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
    - sed -i "s/%%VERSION%%/$CI_COMMIT_SHORT_SHA/" ./public/index.html
  artifacts:
    paths:
      - ./public
  except:
    - schedules

test artifact:
  image: alpine # minimilistic image 5 mb
  cache: {} # diable cache
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html
  except:
    - schedules

test website http:
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
  except:
    - schedules
    
deploy to production:
  stage: deploy production
  cache: {} # diable cache
  environment:
    name: production
    url: http://jawadproduction.surge.sh
  script: 
    - npm install --global surge
    - surge --project ./public --domain jawadproduction.surge.sh
  except:
    - schedules

test production:
  image: alpine
  cache: {} # diable cache
  stage: test production
  script:
    - apk add --no-cache curl
    - curl "http://jawadproduction.surge.sh" | grep -q "Gatsby"
    - curl "http://jawadproduction.surge.sh" | grep -q "$CI_COMMIT_SHORT_SHA"
  except:
    - schedules


deploy to staging:
  stage: deploy staging
  cache: {} # diable cache
  environment:
    name: staging
    url: http://jawadstaging.surge.sh
  script: 
    - npm install --global surge
    - surge --project ./public --domain jawadstaging.surge.sh
  except:
    - schedules

test staging:
  image: alpine
  cache: {} # diable cache
  stage: test staging
  script:
    - apk add --no-cache curl
    - curl "http://jawadstaging.surge.sh" | grep -q "Gatsby"
    - curl "http://jawadstaging.surge.sh" | grep -q "$CI_COMMIT_SHORT_SHA"
  except:
    - schedules


```


# defining variables

we can create variables to be reused again, e.g. staging and production urls

these will be accesses globally in the CI file:

```
variables:
  STAGING_URL: jawadstaging.surge.sh
  PRODUCTION_URL: jawadproduction.surge.sh

```

Full CI code:
```
image: node

stages:
  - build
  - test
  - deploy staging
  - test staging
  - deploy production
  - test production
  - cache

cache: # global cache 
  key: ${CI_COMMIT_REF_SLUG} # we can also define our branch name e.g. master
  paths:
    - node_modules/
  policy: pull

variables:
  STAGING_URL: jawadstaging.surge.sh
  PRODUCTION_URL: jawadproduction.surge.sh

  
update cache:
    stage: cache
    script:
        - npm install
    cache: 
      key: ${CI_COMMIT_REF_SLUG} 
      paths:
       - node_modules/
    only:
      - schedules # this will make the job run only when pipeline is scheduled

build website:
  stage: build
  script:
    - echo $CI_COMMIT_SHORT_SHA
    - npm install
    - npm install -g gatsby-cli
    - gatsby build
    - sed -i "s/%%VERSION%%/$CI_COMMIT_SHORT_SHA/" ./public/index.html
  artifacts:
    paths:
      - ./public
  except:
    - schedules

test artifact:
  image: alpine # minimilistic image 5 mb
  cache: {} # diable cache
  stage: test
  script:
    - grep -q "Gatsby" ./public/index.html
  except:
    - schedules

test website http:
  stage: test
  script:
    - npm install
    - npm install -g gatsby-cli
    - gatsby serve & # this will run this command in background and will release for next command
    - sleep 3 # add a pause
    - curl "http://localhost:9000" | tac | tac | grep -q "Gatsby"  # output of curl will be given as input to grep; tac is a unix program that reads the entire input page, and when grep will run when curl will finish writing
  except:
    - schedules
    
deploy to production:
  stage: deploy production
  cache: {} # diable cache
  environment:
    name: production
    url: http://$PRODUCTION_URL
  script: 
    - npm install --global surge
    - surge --project ./public --domain $PRODUCTION_URL
  except:
    - schedules

test production:
  image: alpine
  cache: {} # diable cache
  stage: test production
  script:
    - apk add --no-cache curl
    - curl "http://$PRODUCTION_URL" | grep -q "Gatsby"
    - curl "http://$PRODUCTION_URL" | grep -q "$CI_COMMIT_SHORT_SHA"
  except:
    - schedules


deploy to staging:
  stage: deploy staging
  cache: {} # diable cache
  environment:
    name: staging
    url: http://$STAGING_URL
  script: 
    - npm install --global surge
    - surge --project ./public --domain $STAGING_URL
  except:
    - schedules

test staging:
  image: alpine
  cache: {} # diable cache
  stage: test staging
  script:
    - apk add --no-cache curl
    - curl "http://$STAGING_URL" | grep -q "Gatsby"
    - curl "http://$STAGING_URL" | grep -q "$CI_COMMIT_SHORT_SHA"
  except:
    - schedules


```

# Manually Triggering jobs/ Manual Deployments

adding ` when: manual` in job waits for the manual intervention of user/admin

add below code in `deploy production` job, this will wait for us to manually accept the job in pipeline view.

```
 when: manual
 allow_failure: false
```

Pipeline  status blocked

![Blocked](https://github.com/jawad1989/GitLab101/blob/master/images/7%20-%20pipeline%20waiting.PNG)

Pipeline waitng for action

![WAiting](https://github.com/jawad1989/GitLab101/blob/master/images/7b.PNG)

Pipeline Running after manual trigger

![Triggered](https://github.com/jawad1989/GitLab101/blob/master/images/7c.PNG)

## References:
https://docs.gitlab.com/ee/ci/yaml/#whenmanual
https://docs.gitlab.com/ee/ci/yaml/#allow_failure


# Merge Requests - Using Branches

**Using Branches:**

* each feature/task/bugfix could be done on a separate branch
* once tested and reviewed, it can be merged back into master

**Branching Models:**
 
 * most known strategy is using gitFlow
 * use any strategy just dont use only one branch
 
 Merge requests - What is a Merge Request?
Merge Requests are a good way to visualize new changes that are about to be made in master

![Merge](https://github.com/jawad1989/GitLab101/blob/master/images/merge%20request%20-%20a.png)

and see the status of the pipeline for a specific branch

![Merge](https://github.com/jawad1989/GitLab101/blob/master/images/merge%20request%20-%20b.png)

but also to give other developers the possibility of giving their feedback regarding a feature/fix before it gets merge into master.
 
![Merge](https://github.com/jawad1989/GitLab101/blob/master/images/merge%20request%20-%20c.png)


# Configure Merge Branch

We can configure our merge branch in many ways i.e.
* Allowing no one to push to master 'Settings->Repositories->protected branch'

![no one push](https://github.com/jawad1989/GitLab101/blob/master/images/merge-allow-no%20one%20to%20push.PNG)


* Chaning merge methods `Setting->General->Merge Methods`

![Fast Forward](https://github.com/jawad1989/GitLab101/blob/master/images/merge-fast-forward-merge.PNG)



## GitLab Registery 

### Useful Resources
* More Eamples can be seen at<br/> [GitLAB CICD](https://docs.gitlab.com/ee/ci/examples/README.html)<br/>
* https://about.gitlab.com/blog/2018/01/22/a-beginners-guide-to-continuous-integration/<br/>
* https://gitlab.com/gitlab-examples<br/>
* Self-Practice: Try JUnit with various test frameworks: https://docs.gitlab.com/ee/ci/junit_test_reports.html <br/>
* https://www.youtube.com/watch?v=l5705U8s_nQ&t=397
