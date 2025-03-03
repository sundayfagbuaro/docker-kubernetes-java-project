pipeline {
    agent any
    tools { 
      maven 'maven-3.9.9' 
      jdk 'jdk-8' 
    }
    stages {
        stage('SCM Checkout') {
            steps {
                script {
                    git branch: 'test_branch', credentialsId: 'git-pat', url: 'https://github.com/sundayfagbuaro/docker-kubernetes-java-project.git'
                
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh """ 
                    cd shopfront
                    mvn clean install
                """
                }
                
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building the image"
                sh 'docker build -t sundayfagbuaro/shopfront:latest .'
            }
        }
    }
}
