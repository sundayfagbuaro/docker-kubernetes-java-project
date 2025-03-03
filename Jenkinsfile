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
        stage('Build The Artifact') {
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
                script{
                    sh """ 
                    cd shopfront
                    echo "Building docker image for shopfront microservice"
                    docker build -t sundayfagbuaro/shopfront:latest .
                    docker image ls
                    """
                }               
            }
        }
        stage ("Push Docker Image to DockerHub") {
            steps {
                    echo "Pushing the built image to docker hub"
                    withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'docker_pwd', usernameVariable: 'docker_user')]) {
                sh 'docker login -u ${docker_user} -p ${docker_pwd}' 
                }
                sh 'docker push sundayfagbuaro/shopfront:latest '
            }
        }
    }
}
