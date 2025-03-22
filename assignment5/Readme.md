# Terraform Setup Guide

## Install Terraform
![Screenshot (353)](https://github.com/user-attachments/assets/4d602a60-7bdf-43c5-8a6c-7bef5d09288b)

## Local.tf
![Screenshot (352)](https://github.com/user-attachments/assets/44e6274d-7dff-4f10-9034-699407ec077b)

## Apply Terraform
![Screenshot (357)](https://github.com/user-attachments/assets/75ca4c22-b11a-4d40-ba58-a972a6986a6c)

## Edit Local.tf
![Screenshot (358)](https://github.com/user-attachments/assets/6cbf60d5-b545-462a-a94f-98577c43b47c)

## Run the File
![Screenshot (360)](https://github.com/user-attachments/assets/84a67312-4f78-4904-9296-c5cbb3df67a4)

## Output
![Screenshot (362)](https://github.com/user-attachments/assets/4a316ba3-16f1-4b30-a7d9-cef75d752f61)

## Jerkin Output
![Screenshot (363)](https://github.com/user-attachments/assets/c602f7fa-4627-4ea8-8231-98ebf2da99cd)

## Console Output
![Screenshot (364)](https://github.com/user-attachments/assets/c2d6777d-3a79-455a-a73b-0d660720da07)

## Pipeline Script
```groovy
pipeline {
    agent any
    
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kannancloud001/spring-framework-petclinic.git']])
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t ikjas/java-maven .'
                }
            }
        }
    }
}
```

