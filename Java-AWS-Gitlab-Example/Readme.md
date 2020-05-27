# 1. introduction
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
  
  
  
 
