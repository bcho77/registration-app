pipeline {
    agent {
        label 'maven'
    }

    stages {
        stage('checkout') {
            steps{
                checkout scm
            }
                 
        }
        stage('Git clone'){
            steps{
                git branch: 'main', url: 'https:https://github.com/bcho77/registration-app.git'
            }
        }
        stage('Build with maven'){
            steps{

                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                sh 'docker build -t registration-app .'
                sh 'docker tag registration-app vaninoel/registration-app:latest'
                sh 'docker images list'

                withCredentials([string(credentialsId: 'GITHUB_CREDENT', variable: 'dockerhub')]) {
                sh 'docker login -u vaninoel -p $dockerhub'
            }
            }
            
        }
        stage('push'){
            steps{
                sh 'docker push vaninoel/registration-app:latest'
            }
        }
        stage(' kubernetes deploy'){
            steps{
                sh 'kubectl apply -f regapp-service.yml'
                sh 'kubectl apply -f regapp-deploy.yml'
            }
        }
    }
    
}
