pipeline {
    agent any

    environment {
        //you must set the following environment variables
        // ORGANIZATION_NAME
        // YOUR_DOCKERHUB_USERNAME (it doesnt matter if you dont have one)

        SERVICE_NAME = "sls-test"
        REPOSITORY_TAG = "${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
    }

    stages {
        stage('Preparation') {
            steps {
                cleanWs()
                git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
            }
        }
        stage('Build') {
            steps {
                sh '''mvn clean package'''
            }
        }

        stage('Build and Push Image') {
            steps {
                //sh 'docker image build -t ${REPOSITORY_TAG} .'
                script {
                    docker.build -t mahesh1101/mahesh-personal-org-sls-test:3 .
                }
            }
        }

        stage('Deploy to Cluster') {
            steps {
                sh 'envsubst < ${WORKSPACE}/pods.yaml | kubectl apply -f -'
            }
        }
    }
}