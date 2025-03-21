pipeline {
    agent { label 'tomcat' }
    tools { 
        maven 'maven'
    }
    environment {
        SONARQUBE_URL = 'http://3.110.90.173:9000'
    }
    stages {
        stage('git checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Sreedhara1993/war-web-project-java.git'
            }
        }
        stage('build') {
            steps {
                sh "mvn clean install"
            }
        }
        stage('sonarqube analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQubeServer') {  // Ensure this matches your SonarQube name
                        sh "mvn sonar:sonar -Dsonar.projectKey=my_project -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                sh "cp /home/ec2-user/war-web-project/target/wwp-1.0.0.war /opt/tomcat/webapps"
            }
        }
    }
    post {
        success {
            echo "CICD pipeline succeeded"
        }
    }
}
