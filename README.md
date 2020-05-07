# jenkins_project

# GOAL :
To create a system in which there are two enviorment testing server and prodution server. When the developer push any changes test/dev branch of git then it will automatically deployed on testing server. The testing/Quality assurance team will confirm these changes and will trigger the merging of dev/test branch with master branch and will be deployed to actual production server.

## Requirements :
1. Docker
  	- image: httpd
2. Jenkins
  	- plugin: github
3. Git/ Github account
4. ngrok

## Pre-requisite:
* Give jenkins user superuser power.
* Add a webhook in your github repo.
* Run ngrok so that github webhooks can work.


## Steps :
There are three jobs *test-deploy* that will deploy test-code to testing server, *master-deploy* that will deploy actual production code to production server and *merge-job* that will merge the dev/test branch to master and push changes to github.

* Devloper will push some new changes (features/hotfixes) for website that needs to be tested to the dev/test branch.
This push operation will automatically trigger *test-deploy* job, that will create a docker container and deploy the test code.

* Quality Assurance/ Testing team will continously test changes made in website on testing server.

* After Quality Assurance team approves new code, then they will manually trigger *merge-job* either using the script or jenkins url.

* Since *merge-job* is upstream of *master-deploy* job it will do following things
    - merge the dev branch to master branch
    - push the changes to github
    - *master-deploy* job will be triggered and new code will be deployed to production server.
	
## Configuration

### 1. Testing 

* Created a job that will clone github's dev/test branch and will deploy that code to new docker container. This job is triggered by using github webhooks, Whenever developer pushes changes to dev/test branch it will trigger this job.
![test-deploy](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/test-deploy-1.png)
![test-deploy-2](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/test-deploy-2.png)

### 2. Quality Assurance

* Created a job that will clone the repo's dev and master branch and will merge the dev branch with master branch, if there is any merge-conflict then it will aborted else changes will be pushed back to github. This job will then trigger the master-deploy job.
![merge-job](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/merge-1.png)
![merge-job](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/merge-2.png)
![merge-job](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/merge-3.png)
![merge-job](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/merge-4.png)

### 3. Production

* Created a job that will clone repo's master branch and will deploy it to the actual production server that is running on docker container. This job will run only when merge-job is successful.
![prod](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/master-deploy-1.png)
![prod](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/master-deploy-2.png)
![prod](https://github.com/harsh-gautam/jenkins_project/blob/master/screenshots/master-deploy-3.png)
