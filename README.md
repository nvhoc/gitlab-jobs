# GITLAB JOBS

## Background
In Microservice Architecture, we have many small services as repositories and the released time is very short. It's very high-load if we do all thing manually. 

The jobs including:
- autotag a repo when there are new code in the master branch
- docker auto push image with new version 
- deploy the service in ec2 using docker

## How to use

1. add .gitlab-ci.yaml in a service's repo
2. add code 
```
stages:
  - tag
  - build
  - deploy
  
include:
  - remote: https://github.com/nvhoc/gitlab-jobs/releases/download/v1.0.0/default.gitlab-ci.tag.yml
  - remote: https://github.com/nvhoc/gitlab-jobs/releases/download/v1.0.0/default.gitlab-ci.docker.yml
  - remote: https://github.com/nvhoc/gitlab-jobs/releases/download/v1.0.0/default.gitlab-ci.ec2-deploy.yml

```
3. define CI&CD variables:

3.1. to include .gitlab-ci.tag.yml:
+ generate a pair of key ssh by ssh-keygen => private_key && public_key 
+ set SSH_PRIVATE_KEY=${private_key}  (text)
+ add public key in "Deploy Keys" at "CI&CD settings" 
+ set GITHUB_EMAIL=${your bot email}
+ set GITHUB_USER=${your username}

3.2. to include .gitlab-ci.docker.yml:
+ set DOCKER_USER_NAME=${your bot username in your docker registry}
+ set DOCKER_PASSWORD=${your bot password in your docker registry}

3.3. to include .gitlab-ci.ec2-deploy.yml:
+ note: the file support for simple deployment with docker-compose. I will add new file for k8s later.
+ set EC2_PEM_KEY=${content of pem file}
+ set EC2_USER=${ec2 username}
+ set EC2_HOST=${ec2 public address}
+ set EC2_DEPLOY_PATH=${ec2 path store docker-compose deployment} find a example at ./example/docker-compose

3.4. to include .gitlab-ci.yarn-build.yml:
+ set BUILD_PATH=${path of folder from building process}

3.5. to include .gitlab-ci.s3-sync.yml:
+ set BUILD_PATH=${path of folder that you want to sync to s3}
+ set S3_DEPLOY_BUCKET=${s3 bucket}