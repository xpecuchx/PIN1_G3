pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    DOCKER_IMAGE_NAME = "federicomas/pin_g3"
  }

  stages {
    stage('Checkout Git Repo') {
      steps {
          git branch: "${env.BRANCH_NAME}", url: "https://github.com/xpecuchx/PIN1_G3"
        }
      }

    stage('Building image') {
      steps {
        script {
          sh "docker build -t ${DOCKER_IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
        }
      }
    }
    
    stage('Run tests') {
      steps {
        sh "docker run ${DOCKER_IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER} npm test"
      }
    }

    stage('Publish Docker Image') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-token', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
            sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
            sh "docker push ${DOCKER_IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
            sh "docker tag ${DOCKER_IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ${DOCKER_IMAGE_NAME}:${env.BRANCH_NAME}-latest"
            sh "docker push ${DOCKER_IMAGE_NAME}:${env.BRANCH_NAME}-latest"
          }
        }
      }
    }
  }
}


    
  

