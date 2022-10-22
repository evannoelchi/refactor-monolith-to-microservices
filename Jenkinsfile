pipeline {
    agent any
    environment{
        DOCKERHUB_CREDS = credentials('DockerHub')
    }
    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
                sh 'ls *'
            }
        }
        stage('Build Image') {
            steps {

		        // Build
                sh 'docker build -t udagram-api-feed ./udagram-api-feed'
                sh 'docker build -t udagram-api-user ./udagram-api-user'
                sh 'docker build -t udagram-frontend ./udagram-frontend'
                sh 'docker build -t udagram-reverseproxy ./udagram-reverseproxy'

                // Tagging
                sh 'docker tag udagram-api-feed evannoelchi/udagram-api-feed:v4a'
                sh 'docker tag udagram-api-user evannoelchi/udagram-api-user:v4a'
                sh 'docker tag udagram-frontend evannoelchi/udagram-frontend:local'
                sh 'docker tag udagram-reverseproxy evannoelchi/udagram-reverseproxy:v4a'
  
            }
        }
        stage('Docker Login') {
            steps {
                
                sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'                
                }
            }
        stage('Docker Push') {
            steps {
		        // Push to Docker Hub

                sh 'docker push evannoelchi/udagram-api-feed:v4a'
                sh 'docker push evannoelchi/udagram-api-user:v4a'
                sh 'docker push evannoelchi/udagram-frontend:local'
                sh 'docker push evannoelchi/udagram-reverseproxy:v4a'


                }
            }
        }
    post {
		always {
			sh 'docker logout'
		}
	 }
    }

