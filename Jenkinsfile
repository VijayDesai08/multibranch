pipeline {
    agent any
    tools {
        maven 'maven-3.9.9'
    }

    stages {
        stage('build war') {
            steps {
                sh '/opt/maven-3.9.9/bin/mvn clean install'
            }
        }

        stage('build image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred'){
                        sh 'docker build -t vijay008/multibranch-yellow:v$BUILD_NUMBER .'
                   }
                }
            }
        }     

        stage('push image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred'){
                        sh 'docker push vijay008/multibranch-yellow:v$BUILD_NUMBER'
                    }
                }
            }
        }

        stage('deploy image') {
            steps {
                
                sh 'docker run --rm --name tommycntr-yellow -d -p 8083:8080 vijay008/multibranch-yellow:v$BUILD_NUMBER'
            }
        }
     }
}
