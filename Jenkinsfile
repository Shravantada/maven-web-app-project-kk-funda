pipeline {
    agent any
    tools {
        maven "maven 3.9.8"
    }
    stages {
        stage('checkout') {
            steps {
                git branch: 'development', credentialsId: '73f708c2-65f4-4bbe-a315-63d249e9cda9', url: 'https://github.com/Shravantada/maven-web-app-project-kk-funda.git'
            }
        }
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('Sonarqube report') {
            steps {
                sh "mvn sonar:sonar"
            }
        }
        stage('deploynexus') {
            steps {
                sh "mvn deploy"
            }
        }
        stage('deploytomcat') {
            steps {
                sshagent(['89dc6853-1723-4891-b1da-8378fa5e566d'])
                {
                sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.109.1.57:/opt/apache-tomcat-9.0.93/webapps"
                }
            }
        }
    }
    post {
  success {
    slackSend (color: '#33ff3c', message: "SUCCESS: Job '${env.JOB_NAME} [$env.BUILD_NUMBER}]' (${env.BUILD_URL})",channel: '#shravan_reddy')
        }
        failure {
            slackSend (color: '#fff933', message: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})",channel: '#shravan_reddy')
        }
    }
}
