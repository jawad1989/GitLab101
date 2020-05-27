# Table of Contents
1. Introduction
2. Clone the project in intelliJ
3. Build the project using Gradle
4. Testing using postman
5. Build/Test Java project locally
6. Create first CICD file using intelliJ
7. Create a smoke test
8. Elastic Bean Stalk
9. Automate - deploy to AWS using Gitlab
10. Upload jar file to beanstalk
11. Getting info API to run
12. Testing the app version in deploy stage
13. Test code using PMD
14. Automate Test code using pmd
15. Integration JUnit with Gitlab CI
16. Api testing using newman/postman
****************
# 1. Introduction
In this example we will use  a JAVA application in GITLAB to perform CICD

# 2. Clone the project in intelliJ
 https://gitlab.com/jawadxiv/cars-api/-/tree/master
 
# 3. Build the project using Gradle
  * Once Intellij is running with the GIT project
  * on the right side you will see gradle, click `CARS-API->TASKS->APPLICATION->BOOTRUN`
  * This will create a local running build 
  
  ![intelli](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/intelli-j.PNG)
  
 
# 4. Testing using postman
 * Download the postman collection and import them in post man<br/>
 [Download collection](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/Postman%20collection.rar)

  ## 4.1. Add a car
  
  ![Add Car](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/add-car.PNG)
  
  ## 4.2. Get all Cars
  
  ![get all cars](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/get-all-cars.PNG)
  
# 5. Build/Test Java project locally

we can run below commands in intelliG terminal for building the project again

```
gradlew clean
gradlew build
```


# 6. Create first CICD file using intelliJ

in this task we will build the file on gitlab using gitlab-ci 

 * Create a gitlab-ci file  in intelliJ
 
  ```
  stages:
  - build

build:
  stage: build
  image: openjdk:12-alpine
  script:
    - ./gradlew build
  artifacts:
    paths:
      - ./build/libs/

  ```
  
  * Commit the file
  * Push the file(add remote origin in intelliJ)
  * This will run the pipeline in Gitlab
  
  ![build pipleine](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/gitlab-pipeline.PNG)
  
  * check the artifacts for build file (.jar)
  
  ![build file](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/artifacts.PNG)
  
 # 7. Create a smoke test
   
   >Smoke Test: simple test to check if something is working
   
   * Add below job in gitlab-ci
    ***its an alpine image so it doesnt comes with curl, so we have to install it***
    ***localhost:5000 is base url, we can check this in postman***
    ***curl 0 means success, 1 means failed***
   ```
  smoke test:
  stage: test
  image: openjdk:12-alpine
  before_script:
    - apk --no-cache add curl
  script:
    - java -jar ./build/libs/cars-api.rar & # to run in background
    - sleep 30
    - curl http://localhost:5000/actuator/health | grep "UP"
   ```
   
   * Commit and push the file from intelliJ
   
   * in Gitlab pipeline will start once the push is complete
   
   ![smoketest-pipeline](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/pipleine-smoke.PNG)
  
  * Pipeline is complete, smoke test is successfull
  
  ![smoke-success](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/pipeline-success.PNG)
 
# 8. Elastic Bean Stalk
AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS.

* sign in to yout AWS console
* Select Elastic BeanStalk in services
* Create Application
* Name the app

![name the app](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-a.PNG)

* select java in environment

![select java](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-b.PNG)

* Start with sample code

![beanstalk](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-creating-sample.PNG)

* once the env is created 

![Env created](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-d.PNG)

* Sample app is running

![Sample App](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-c.PNG)

* upload the jar file on beanstalk

![jar](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-e.PNG)

![upload](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-f.PNG)

* now duplicate the localhost environment by clicking the gear icon and pressing duplicate

![duplicate](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-g.PNG)

* get the url from beanstalk and update teh initial and current url

![updateurl](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-g.PNG)

* Test the production env from postman
this will deploy our app to beanstalk/aws clous and we are testing it using postman
![test](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/beanstalk-i.PNG)

# 9. Automate - deploy to AWS using Gitlab

gitlab cant directly upload to `elastic beanstalk`, we will use `aws s3`. So we will upload jar file to s3 from gitlab and deploy it on production environment.

> AWS CLI docs : https://docs.aws.amazon.com/cli/latest/reference/s3/

```
deploy to aws:
  stage: deploy
  image:
    name: banst/awscli # un official docker image
    entrypoint: [""]
  script:
    - aws configure set region us-east-1
    - aws s3 cp ./build/libs/cars-api.jar s3://$S3_BUCKET/cars-api.jar

```

### * Create a S3 Bucket

Bucket Name: gitlab-beanstalk
![Create Bucket](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/create-bucket.PNG)

### Gitlab Group Settings
   
  *  Create a new variable with S3_bucket as name
   
   > un check the protected check box
   
   ![bucket variable](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/gitlab-add-variable.PNG)


### Upload file to AWS S3 using Gitlab

* in AWS , goto IAM
* Create a new user `gitlabci`
* assign policy `AmazonS3FullAccess`
![se](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/s3-policy-iam.PNG)
* copy the access key and secret
![s3](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/iam-user-summary.PNG)

* in gitlab create variables

AWS_ACCESS_KEY_ID

![key](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/git-var.PNG)

AWS_SECRET_ACCESS_KEY

![key](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/git-var-2.PNG)

* Commit and Push the Gitlab-ci
 this will run the pipeline where our code is being built as a jar and pushed on our s3 bucket
 
 ![upload](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/pipeline-upload-to-s3.PNG)
 
* pushed successfully to S3

![S3-success](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/s3-sucess-upload.PNG)

Final pipeline
```
stages:
  - build
  - test
  - deploy

variables:
  ARTIFACT_NAME: cars-api.jar

build:
  stage: build
  image: openjdk:12-alpine
  script:
    - ./gradlew build
  artifacts:
    paths:
      - ./build/libs/

smoke test:
  stage: test
  image: openjdk:12-alpine
  before_script:
    - apk --no-cache add curl
  script:
    - java -jar ./build/libs/$ARTIFACT_NAME & # to run in background
    - sleep 30
    - curl http://localhost:5000/actuator/health | grep "UP"

deploy to aws:
  stage: deploy
  image:
    name: banst/awscli # un official docker image
    entrypoint: [""]
  script:
    - aws configure set region us-east-1
    - aws s3 cp ./build/libs/$ARTIFACT_NAME s3://$S3_BUCKET/$ARTIFACT_NAME

```
# 10. Upload jar file to beanstalk

we have to do below two steps:
 1. create a new app version
 we will using pre defined variables to generate a unique name/id for our version
 
 2. update the env with our new version
 
 
 in this task we will use AWS elasticbeanstalk commands for IAM
 
 https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/
 
 ```
 deploy to s3:
  stage: deploy
  image:
    name: banst/awscli # un official docker image
    entrypoint: [""]
  script:
    - aws configure set region us-east-1
    - aws s3 cp ./build/libs/$ARTIFACT_NAME s3://$S3_BUCKET/$ARTIFACT_NAME
    - aws elasticbeanstalk create-application-version --application-name $APP_NAME --version-label $CI_PIPELINE_IID --source-bundle S3Bucket=$S3_BUCKET,S3Key=$ARTIFACT_NAME
    - aws elasticbeanstalk update-envirionment --application-name $APP_NAME --environment-name "production" --version-label $CI_PIPELINE_IID

```

### complete gitlab ci code:
```
variables:
  ARTIFACT_NAME: cars-api-v$CI_PIPELINE_IID.jar # The unique ID of the current pipeline scoped to project
  APP_NAME: cars-api

stages:
  - build
  - test
  - deploy

build:
  stage: build
  image: openjdk:12-alpine
  script:
    - ./gradlew build
    - mv ./build/libs/cars-api.jar ./build/libs/$ARTIFACT_NAME
  artifacts:
    paths:
      - ./build/libs/

smoke test:
  stage: test
  image: openjdk:12-alpine
  before_script:
    - apk --no-cache add curl
  script:
    - java -jar ./build/libs/$ARTIFACT_NAME & # to run in background
    - sleep 30
    - curl http://localhost:5000/actuator/health | grep "UP"

deploy to aws:
  stage: deploy
  image:
    name: banst/awscli # un official docker image
    entrypoint: [""]
  script:
    - aws configure set region us-east-1
    - aws s3 cp ./build/libs/$ARTIFACT_NAME s3://$S3_BUCKET/$ARTIFACT_NAME
    - aws elasticbeanstalk create-application-version --application-name $APP_NAME --version-label $CI_PIPELINE_IID --source-bundle S3Bucket=$S3_BUCKET,S3Key=$ARTIFACT_NAME
    - aws elasticbeanstalk update-envirionment --application-name $APP_NAME --environment-name "production" --version-label $CI_PIPELINE_IID

```
https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/


###  Now commit and push the gitlab ci file
 Pipeline will fail, reason is our use does not have permission to exectue elastic bean stalk commands
 ```
 $ aws s3 cp ./build/libs/$ARTIFACT_NAME s3://$S3_BUCKET/$ARTIFACT_NAME
 upload: build/libs/cars-api-v8.jar to s3://gitlab-beanstalk/cars-api-v8.jar
 $ aws elasticbeanstalk create-application-version --application-name $APP_NAME --version-label $CI_PIPELINE_IID --source-bundle S3Bucket=#S3_BUCKET,S3Key=$ARTIFACT_NAME
 An error occurred (AccessDenied) when calling the CreateApplicationVersion operation: User: arn:aws:iam::402953086859:user/gitlabci is not authorized to perform: elasticbeanstalk:CreateApplicationVersion on resource: arn:aws:elasticbeanstalk:us-east-1:402953086859:applicationversion/cars-api/8
Running after_script
00:01
Uploading artifacts for failed job
00:02
 ERROR: Job failed: exit code 255
 ```
 ###  Assign permissions to user and re run the job
  * Goto IAM
  * select User
  * press `Add permission`
  * assign the permission
  
  ![permission](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/elastic-bean-permission-iam.PNG)
 
 ![S3](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/success-pipeline.PNG)
 
 ![S3](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/success-env.PNG)
 
 ![S3 success](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/success-s3.PNG)

# 11. Getting info API to run
we will update our code to get the info like env version from API in postman
below are the changes that need to be done in gitlab ci

***build***
```
build:
  stage: build
  image: openjdk:12-alpine
  script:
    - sed -i "s/CI_PIPELINE_IID/$CI_PIPELINE_IID/" ./src/main/resources/application.yml
    - sed -i "s/CI_COMMIT_SHORT_SHA/$CI_COMMIT_SHORT_SHA/" ./src/main/resources/application.yml
    - sed -i "s/CI_COMMIT_BRANCH/$CI_COMMIT_BRANCH/" ./src/main/resources/application.yml
    - ./gradlew build
    - mv ./build/libs/cars-api.jar ./build/libs/$ARTIFACT_NAME
  artifacts:
    paths:
      - ./build/libs/
```

***Deploy***
```
deploy to aws:
  stage: deploy
  image:
    name: banst/awscli # un official docker image
    entrypoint: [""]
  before_script:
    - apk --no-cache add curl
    - apk --no-cache add jq #parse json in cli
  script:
    - aws configure set region us-east-1
    - aws s3 cp ./build/libs/$ARTIFACT_NAME s3://$S3_BUCKET/$ARTIFACT_NAME
    - aws elasticbeanstalk create-application-version --application-name $APP_NAME --version-label $CI_PIPELINE_IID --source-bundle S3Bucket=$S3_BUCKET,S3Key=$ARTIFACT_NAME
    - CNAME=$(aws elasticbeanstalk update-environment --application-name $APP_NAME --environment-name "CarsApi-env" --version-label=$CI_PIPELINE_IID | jq '.CNAME' --raw-output)
    - sleep 45
    - curl http://$CNAME/actuator/info | grep $CI_PIPELINE_IID
    - curl http://$CNAME/actuator/health | grep "UP"
```

after pipelines gets complete you can view the status in postman:
![postman](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/info-api.PNG)

# 12. Testing the app version in deploy stage

below code will grep the app version and status from deployed pipeline
```
deploy to aws:
  stage: deploy
  image:
    name: banst/awscli # un official docker image
    entrypoint: [""]
  before_script:
    - apk --no-cache add curl
    - apk --no-cache add jq #parse json in cli
  script:
    - aws configure set region us-east-1
    - aws s3 cp ./build/libs/$ARTIFACT_NAME s3://$S3_BUCKET/$ARTIFACT_NAME
    - aws elasticbeanstalk create-application-version --application-name $APP_NAME --version-label $CI_PIPELINE_IID --source-bundle S3Bucket=$S3_BUCKET,S3Key=$ARTIFACT_NAME
    - CNAME=$(aws elasticbeanstalk update-environment --application-name $APP_NAME --environment-name "CarsApi-env" --version-label=$CI_PIPELINE_IID | jq '.CNAME' --raw-output)
    - sleep 45
    - curl http://$CNAME/actuator/info | grep $CI_PIPELINE_IID
    - curl http://$CNAME/actuator/health | grep "UP"

```

![pipelin](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/pipeline-up.PNG)


# 13. Test code using PMD

we will test the code using PMD library that checks for code like un assigned variables, using system println and returns the report by failing the build.

Source: https://pmd.github.io/latest/pmd_rules_java_bestpractices.html#systemprintln


Run the test

```
gradlew pmdTest pmdMain
```

![add](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/pmd-systemprintln.PNG)

Code failed

![Code Failed](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/code_failed.PNG)

Report in browser
![report](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/pmd_report.PNG)


# 14. Automate Test code using pmd

in this task we will create a new job and save the pmd report as an artifact
```
code quality:
  stage: test
  image: openjdk:12-alpine # we are using gradle thats why we have to use jdk
  script:
    - ./gradlew pmdMain pmdTest
  artifacts:
    when: always # this artifacts wil be saved always in success or failed
    paths:
      - build/reports/pmd
```

now as we already have rules updated with checking system print ln, once we run our pipeline the new job will fail and artifact will be placed on location:

![failed](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/code_quality_failed.PNG)

![pipeline](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/code-quality-failed.PNG)

artifact placed

![artifact](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/artifact_updated.PNG)

Report PMD
![pmd report](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/artifact_uploaded.PNG)


# 15. Integration JUnit with Gitlab CI

we can run unit test in inteelliJ by runnin `gradlew test` we will add a new job in gitlab ci

this will run the defined tests and set the reult in artifacts

```
unit tests:
  stage: test
  image: openjdl:12-alpine
  script:
    - ./gradlew test
  artifacts:
    when: always
    paths:
      - build/reports/tests
    reports:
      junit: build/test-results/test/*.xml
```

> we can also view junit results in gitlab ci pipeline view

![unit test](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/unit%20tests.PNG)

![junit a](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-junit.PNG)

![junit b](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-junit-b.PNG)

![junit c](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-juni-c.PNG)


## in case of junit failure 
we will see the below report
![report](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/junit-failed.PNG)



# 16. Api testing using newman/postman

in this stage we wwill add jobs to run our potman tests from gitlab-ci
1) run postman, goto tests tab for get all cars
2) from right side Press "status code 200" test, this will create a test in test section
3) export the collection and save in the project folder
4) export the enviornmnet and save in the project folder

![postman](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-a.PNG)

![export](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-a-b.PNG)

5) add a new job and stage(postdeploy)

this will use `newman` to test postman collection from cli and export the reports

```
api testing:
  stage: postdeploy
  image:
    name: vdespa/newman # this is a docker image with newman installed
    entrypoint: [""]
  script:
    - newman --version
    - newman run "Cars API.postman_collection.json" --environment Production.postman_environment.json --reporters cli,htmlextra,junit --reporter-htmlextra-export "newman/report.html" --reporter-junit-export "newman/report.xml"
  artifacts:
    when: always
    paths:
      - newman/
    reports:
      junit: newman/report.xml
```

6) use gitlab pages to export/view the `newman` report

```
pages:
  stage: publishing
  script:
    - mkdir public
    - mv newman/report.html public/index.html
  artifacts:
    paths:
      - public
```

7) once pipleline is completed, you will notice a new job will automatically be created

![auto job](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-c.PNG)

goto tests and goto deploy to see the job type
![job new](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-d.PNG)

this will also create a gitlab page for our newman report
![gitlab pages](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/gitlabpages.PNG)

![pages](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-e.PNG)

![pages](https://github.com/jawad1989/GitLab101/blob/master/Java-AWS-Gitlab-Example/misc/1-f.PNG)



**************
# Resources
https://docs.gitlab.com/ee/user/project/pages/
https://www.youtube.com/watch?v=Qlvbc6kIBOk
https://github.com/postmanlabs/newman
https://pmd.github.io/latest/pmd_rules_java_bestpractices.html#systemprintln
