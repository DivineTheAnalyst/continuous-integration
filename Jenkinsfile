pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'SonarScanner'
        MAVEN_HOME = tool 'maven1'
    }
    tools {
        jdk 'OpenJDK-17'
    }
    stages {
        stage ('SCM') {
            steps {
                checkout scm
            }
        }
        stage ('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }
        stage ('Sonarqube Analysis') {
            steps {
                script {
                    sh '''
                        maven1 clean verify sonar:sonar \
                            -Dsonar.projectKey=calculator-jenkins \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=sonar_project
                    '''
                }
            }
        }
    }
}