pipeline {
  agent any

  environment {
    DOCKER_IMAGE_NAME = "agustinapecuch/pin1g3:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
    GITHUB_REPO = "https://github.com/xpecuchx/PIN1_G3"
  }

  stages {
    stage('Checkout Git Repo') {
      steps {
          git branch: "${env.BRANCH_NAME}", url: "${GITHUB_REPO}"
      }
    }

    stage('Building image') {
      steps {
        script {
          sh "docker build -t ${DOCKER_IMAGE_NAME} ."
        }
      }
    }

    stage('Publish Docker Image') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-token', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
            sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
            sh "docker push ${DOCKER_IMAGE_NAME}"
            sh "docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_IMAGE_NAME}-latest"
            sh "docker push ${DOCKER_IMAGE_NAME}-latest"
          }
        }
      }
    }
  }
}
