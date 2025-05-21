pipeline {
    agent {
        docker {
            image 'maven:3.8.6-openjdk-17'
        }
    }

    environment {
        IMAGE_NAME = 'spring-petclinic'
        DOCKERHUB_USER = 'your-dockerhub-username' // optional if you want to push
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/spring-projects/spring-petclinic.git'
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
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Optional: Docker Push') {
            when {
                expression { return env.DOCKERHUB_USER != null }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                        docker tag $IMAGE_NAME $USER/$IMAGE_NAME
                        docker push $USER/$IMAGE_NAME
                    '''
                }
            }
        }
    }
}
