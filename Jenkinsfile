pipeline {
    agent any
    tools { 
      maven 'maven-3.9.9' 
      jdk 'jdk-21' 
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
    }
}
