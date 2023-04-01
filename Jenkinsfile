pipeline {
    agent any
    tools {
        maven 'MAVEN'
        jdk 'JAVA'
    }
    environment {
    DOCKERHUB_CREDENTIALS= credentials('dockerhubcreds')
  }
    stages {
     stage ('Build the source code') {
      steps {
        sh 'mvn clean package'
      }
    }

     stage('Copy Artifact') {
      steps {
                 sh 'pwd'
                 sh 'cp -r target/*.jar docker'
           }
        }

     stage('Docker Build') {
      steps {
                    sh 'cd docker'
                    sh 'pwd'
                    sh 'docker build 7989766/devops-project:$BUILD_NUMBER .'
            }
        }

     stage('Push the image to docker hub image registry') {
      steps {
                    sh 'docker login'
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    echo "login successful!"
            }
        }

     stage('Push Image to Docker Hub') {
      steps{
                    sh 'docker push 7989766/devops-project:$BUILD_NUMBER'
                    echo 'Push Image Completed'
      }
    }
    } 
    post{
    always {
      sh 'docker logout'
    }
  }
    }
