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
        stage('Build Artifact') {
            steps {
                script {
                    sh """ 
                    cd ${build_directory}
                    mvn clean install
                """
                }
                
            }
        }
        stage('Build Docker Image') {
            steps {
                script{
                    sh """ 
                    cd ${build_directory}
                    echo "Building docker image for ${build_directory} microservice"
                    docker build -t sundayfagbuaro/${build_directory}:latest .
                    docker image ls
                    """
                }               
            }
        }
        stage ("Push Docker Image to DockerHub") {
            steps {
                    echo "Pushing the built image for ${build_directory} to docker hub"
                    withCredentials([usernamePassword(credentialsId: 'docker-pat', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]) {
                sh 'docker login -u ${docker_user} -p ${docker_pass}' 
                }
                sh 'docker push sundayfagbuaro/${build_directory}:latest '
            }
        }

        stage('Deploy container to Docker Host') {
            steps {
                echo "Deploying container to docker host"
                script {
                    sshagent(['remote-docker-host']) {
                    sh """ ssh -tt -o StrictHostKeyChecking=no bobosunne@192.168.1.158 << EOF
                        
                        docker run -d -p ${host_port}:${container_port} --name ${build_directory} sundayfagbuaro/${build_directory}:latest 
                        exit
                        EOF"""                    
                }
                
                }
            }
        }
    }
}

