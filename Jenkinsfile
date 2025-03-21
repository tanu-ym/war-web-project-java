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
            when {
                expression { return false }  // This will completely skip the stage
            }
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {  
                        sh "mvn sonar:sonar -Dsonar.projectKey=sonar_project_1 -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONAR_TOKEN}"
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
