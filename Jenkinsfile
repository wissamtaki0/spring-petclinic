pipeline {
    agent {
        docker {
            image 'maven:3.8.6-eclipse-temurin-17'
        }
    }

    environment {
        IMAGE_NAME = 'spring-petclinic'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/wissamtaki0/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
    }
}
