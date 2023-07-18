pipeline {
    agent any
    /*tools {
        maven 'maven_3_8_7'
    }*/
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/WetieF/devops-automation']])
                bat 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t fwatie/devops-integration .'
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        bat 'docker login -u fwatie -p ${dockerhubpwd}'
                    }
                    bat 'docker push fwatie/devops-integration'
                }
            }
        }
    }
}