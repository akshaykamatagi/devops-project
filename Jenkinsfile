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


     stage('Build and Push Image to Docker Hub') {
      steps{
                 script {         
                     def customImage = docker.build('7989766/devops-project:${env.BUILD_NUMBER}', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
      }
    }
    } 
    post{
    always {
      sh 'docker logout'
    }
  }
    }
