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
  - remote: https://raw.githubusercontent.com/nvhoc/gitlab-jobs/master/.gitlab-ci.tag.yml
  - remote: https://raw.githubusercontent.com/nvhoc/gitlab-jobs/master/.gitlab-ci.docker.yml
  - remote: https://raw.githubusercontent.com/nvhoc/gitlab-jobs/master/.gitlab-ci.ec2-deploy.yml
```

