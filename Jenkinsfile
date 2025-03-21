pipeline {
    agent any

    tools{
        java 'jdk-8'
        maven 'maven-3.9.9'
    }

    stages {
        stage('SCM Checkout') {
            steps {
                echo 'Checking Out Repo From Git'
                git branch: 'docker_compose', 
                credentialsId: 'git_cred', 
                url: 'https://github.com/sundayfagbuaro/docker-kubernetes-java-project.git'
            }
        }
        stage('Build The Applications') {
            steps{
                echo "Building The Applications"
                sh "cd shopfront"
                sh "mvn clean install"

            }
        }
    }
}

