pipeline {

    agent any

    environment {

        REPO_NAME = "neeraj91"
        IMAGE_NAME = "${JOB_NAME}"
        IMAGE_TAG = "v1.${BUILD_NUMBER}"
    }

    stages {

        stage('Build') {
            
            steps {
                
                sh '''

                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${REPO_NAME}/${IMAGE_NAME}:${IMAGE_TAG}
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${REPO_NAME}/${IMAGE_NAME}:latest

                '''
            }

        }

        stage ('Docker Push') {

          steps {
            
               withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERVARS', usernameVariable: 'DOCKERNAME')]) {
            
                 sh '''

                   echo $DOCKERVARS | docker login -u $DOCKERNAME --password-stdin
                   docker push ${REPO_NAME}/${IMAGE_NAME}:${IMAGE_TAG}
                   docker push ${REPO_NAME}/${IMAGE_NAME}:latest

                '''
                }
            }
        }

        stage('Deploye') {

          steps {

            sh 'echo Deploy'
          }

        }
    }

}
