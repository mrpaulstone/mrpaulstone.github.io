---
title: "Setting up a CI/CD pipeline using Gitlab, Terraform and AWS"
date: 2022-05-12T19:45:40+08:00
draft: false
---

I recently setup a CI/CD pipeline using Gitlab Terraform and AWS.
This post explains the various parts.
<!--more-->




Gitlab Pipelines
To setup a pipeline in Gitlab, you use a file called .gitlab-ci.yml.

The Gitlab pipeline file contents explained:

        default:
        image:
            name: hashicorp/terraform:latest
            entrypoint:
            - /usr/bin/env
            - "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

        before_script:
            - terraform init
        cache:
            key: terraform
            paths:
            - .terraform

'default' sets up the standard used parts across the rest of the instructions
'entrypoint' provides some basic commands to run automatically after the image starts

'before_script' runs before each script section

'cache:paths' choose which files or directorys to cache
'cache:key' gives each cache a key, so the cache can be re-used. Basically the way the cache is setup here, the terraform init command is run once and the output of that is used for future steps.




        stages:
        - stg_validate
        - stg_plan
        - stg_apply
        - stg_destroy


        jb-tf-validate:
        stage: stg_validate
        script:
        - echo " Validating my terraform code"
        - pwd
        - ls
        - terraform validate
        except:
            refs:
            - master
        jb-tf-plan:
        stage: stg_plan
        script:
            - echo "Generating terraform plan and output to file 'plan' "
            - terraform plan --out plan
        artifacts:
            paths:
            - plan

        jb-tf-apply:
        stage: stg_apply
        script:
            - echo "Spinning up resources via terraform apply step"
            - terraform apply --auto-approve plan
            - terraform state list
        when: always
        allow_failure: false
        artifacts:
            paths:
            - terraform.tfstate

        jb-tf-destroy:
        stage: stg_destroy
        script:
            - echo "Destroying resources"
            - terraform destroy --auto-approve
        when: manual
        only:
            refs:
            - develop



...more content coming soon..