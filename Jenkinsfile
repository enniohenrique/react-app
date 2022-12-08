node {
    checkout scm
}
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh "docker build -t ennio/react-app ."
                sh "docker tag ennio/react-app ennio/react-app:$BUILD_NUMBER"
            }
        }
        stage('Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
			}
		}
        stage('Push') {
			steps {
				sh 'docker push ennio/react-app:$BUILD_NUMBER'
			}
		}
        stage('Deploy Local') {
            steps {
                sh 'docker pull ennio/react-app:$BUILD_NUMBER'
                sh 'docker-compose up -d'
            }
        }
    }        
    post {
        always {
            sh 'docker logout'
        }
    }
}
