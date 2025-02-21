pipeline {
    agent any
    tools {
        nodejs "node16"
    }

    environment {
        FIREBASE_TOKEN = credentials('FIREBASE_TOKEN');
        ENV_FILE_DEST = 'apps/conv-learning-manager/src/environments/environment.ts'
        ENV_FILE_DEST_PROD = 'apps/conv-learning-manager/src/environments/environment.prod.ts'
        }
    stages {
        
        stage('Build the project') { 
            steps {
                sh 'npm install --legacy-peer-deps' 
            }
        }

        stage('Deploy to Enabel') { 
            steps {
                withCredentials([file(credentialsId: 'enabel-prod-environment-file', variable: 'ENV_FILE')]) {
                // some block
                sh 'mkdir -p apps/conv-learning-manager/src/environments'
                sh 'cat ${ENV_FILE} > ${ENV_FILE_DEST}'
                sh 'cat ${ENV_FILE} > ${ENV_FILE_DEST_PROD}'
                sh 'firebase use enabel-elearning'
                sh 'firebase deploy --token ${FIREBASE_TOKEN} --only hosting' 
        }
            }
        }

        stage('Deploy to Elewa Production') { 
            steps {
                withCredentials([file(credentialsId: 'elewa-prod-environment-file', variable: 'ENV_FILE')]) {
                // some block
                sh 'mkdir -p apps/conv-learning-manager/src/environments'
                sh 'cat ${ENV_FILE} > ${ENV_FILE_DEST}'
                sh 'cat ${ENV_FILE} > ${ENV_FILE_DEST_PROD}'
                sh 'firebase use elewa-conv-learning-prod'
                sh 'firebase deploy --token ${FIREBASE_TOKEN} --only hosting' 
        }
            }
        }

        stage('Deploy to Farmbetter') { 
            steps {

                withCredentials([file(credentialsId: 'farmbetter-prod-environment-file', variable: 'ENV_FILE')]) {
                // some block
                
                sh 'mkdir -p apps/conv-learning-manager/src/environments'
                sh 'cat ${ENV_FILE} > ${ENV_FILE_DEST}'
                sh 'cat ${ENV_FILE} > ${ENV_FILE_DEST_PROD}'
                sh 'firebase use farmbetter-prod'
                sh 'firebase target:apply hosting convl-manager farmbetter-prod'
                sh 'firebase deploy --token ${FIREBASE_TOKEN} --only hosting' 
                }
            }
        }
    }
}