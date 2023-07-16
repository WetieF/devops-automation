pipeline {
    agent any
    /*tools {
        maven 'maven_3_8_7'
    }*/
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/WetieF/devops-automation']])
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t fwatie/devops-integration .'
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u fwatie -p ${dockerhubpwd}'
                    }
                    sh 'docker push fwatie/devops-integration'
                }
            }
        }
    }
}