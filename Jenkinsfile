pipeline {
  agent any
  stages {
    stage('Cloning') {
      parallel {
        stage('Cloning') {
          steps {
            sh ' sh\'rm -rf go-web\''
          }
        }
        stage('Removing older folder') {
          steps {
            sh 'rm -rf go-web'
          }
        }
        stage('Cloning from github') {
          steps {
            sh 'git clone https://github.com/aishwaryashinde00/go-web.git'
          }
        }
      }
    }
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'WEB_IMAGE_NAME="aishwaryashinde/go-web:${BUILD_NUMBER}"'
          }
        }
        stage('Docker Build') {
          steps {
            sh 'docker build -t aishwaryashinde/go-web:${BUILD_NUMBER} .'
          }
        }
        stage('Docker Login') {
          steps {
            sh 'docker login -u aishwaryashinde -p ${DOCKER_HUB}'
          }
        }
        stage('Docker Push') {
          steps {
            sh 'docker push aishwaryashinde/go-web:${BUILD_NUMBER} '
          }
        }
      }
    }
    stage('Deployment') {
      parallel {
        stage('Deployment') {
          steps {
            sh 'WEB_IMAGE_NAME="aishwaryashinde/go-web:${BUILD_NUMBER}"'
          }
        }
        stage('Create Deployment') {
          steps {
            sh 'kubectl create -f ${WORKSPACE}/go-web.yaml || true'
          }
        }
        stage('Set Image') {
          steps {
            sh 'kubectl set image deployment/go-web go-web=aishwaryashinde/go-web:${BUILD_NUMBER} --kubeconfig /var/lib/jenkins/.kube/config'
          }
        }
      }
    }
  }
}