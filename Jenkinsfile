pipeline {
    agent any
    tools {
        maven 'maven 3.9.8'
    }
    stages {
        stage('checkout') {
            steps {
                git branch: 'development', credentialsId: '73f708c2-65f4-4bbe-a315-63d249e9cda9', url: 'https://github.com/Shravantada/maven-web-app-project-kk-funda.git'
            }
        }
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('sonarqube') {
            steps {
                sh "mvn sonar:sonar"
            }
        }
        stage('deploy-nexus') {
            steps {
                sh "mvn deploy"
            }
        }
        stage('deploy-tomcat') {
            steps {
                sshagent(['8c6dba63-3d00-4bcc-97a2-5cf32d3af350']) {
                    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.110.224.131:/opt/apache-tomcat-9.0.93/webapps/"
                }
            }
        }
    }
    post {
        success {
            slackSend (color: '#00FF00', message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            slackSend (color: '#FF0000', message: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
}
