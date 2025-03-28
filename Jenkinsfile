pipeline {
    agent { label 'slave-1' }
    
    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {     
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn package"
            }
        }

        stage('Publish Artifacts to Nexus') {
            steps {
                withMaven(maven: 'maven3') {
                    sh "mvn deploy"
                }
            }
        }

        stage('Build and Tag Docker Image') {
            steps {
                sh "docker build -t my-app:latest ."
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_PASS')]) {
                    sh "echo \$DOCKER_PASS | docker login -u pramodkumar054--password-stdin"
                    sh "docker tag my-app:latest pramodkumar054/my-app:latest"
                    sh "docker push pramodkumar054/my-app:latest"
                }
            }
        }
    }
}
