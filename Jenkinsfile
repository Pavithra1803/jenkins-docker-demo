pipeline {
    agent any

    environment {
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
            mail to: 'pavithranamala0906@gmail.com',
            subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Build ${env.BUILD_NUMBER} failed."
        }
    }
}