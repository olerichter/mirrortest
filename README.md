# mirror git repo via deployment for gitlab-CI

[![build status](https://code.ini.uzh.ch/orichter/ci-mirror-tools/badges/master/build.svg)](https://code.ini.uzh.ch/orichter/ci-mirror-tools/commits/master)

This script is made for easy mirroring of git repositories from Gitlab to any other git service

The mirror has to be read only!

## usage:

to use the git mirroring just add to your .gitlab-ci.yml
```
    mirror:github:
        stage: deploy
        image: ubuntu:latest
        script:
        - "apt-get update"
        - "apt-get install git -y"
        - "git clone https://code.ini.uzh.ch/orichter/ci-mirror-tools.git deploy_github"
        - "cd deploy_github"
        - "bash mirror.sh"
```

and configure the remote as a "Secret Variable" at Settings->CI/CD Pipelines->Secret Variables:
```
    MIRROR_GIT_URL => https://USERNAME:ACCESSTOCKEN@github.com/inincs/REPOSITORY.git
```
If you are using GIT LFS add a second variable
```
    MIRROR_LFS => True
```

## Be aware:
the deployment is only excecuted when building works, so when you have build or test jobs and you want the deployment to run even on failure:  add `when: always` to the `mirror: job`

the mirror script will **overwrite all changes** made to the mirror so make it **read-only**!
