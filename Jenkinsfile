pipeline {
//     this tells jenkins where to run the pipeline-like gitlab runner
    agent any
        docker {
            image 'maven:3.9.9-eclipse-temurin-17'
        }

    environment {
    //jenkins provide some vars like BUILD_NUMBER,JOB_NAME.
        BUILD_TAG = "build-${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Pavithra1803/jenkins-docker-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Tag Build') {
            steps {
                sh 'cp target/*.jar target/app-${BUILD_NUMBER}.jar'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        failure {
            mail to: 'i48615886@gmail.com',
            subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Build ${env.BUILD_NUMBER} failed."
        }
    }
}