# Table of Contents
1. Introduction
2. Clone the project in intelliJ
3. Build the project using Gradle
4. Testing using postman
5. Build/Test Java project locally
6. Create first CICD file using intelliJ
7. Create a smoke test

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
 
# 7. Elastic Bean Stalk
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

# 8. Automate - deploy to AWS using Gitlab

gitlab cant directly upload to `elastic beanstalk`, we will use `aws s3`. So we will upload jar file to s3 from gitlab and deploy it on production environment.

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
