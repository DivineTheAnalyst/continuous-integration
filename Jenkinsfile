pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'SonarScanner'
        MAVEN_HOME = tool 'maven1'
        DOCKER_IMAGE_NAME = 'calculator-app'
        DOCKER_TAG = 'latest'
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
                        ${MAVEN_HOME}/bin/mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=calculator-jenkins \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=squ_2cf37c7961b9ff436319884ea0efe7efffc26925
                    '''
                }
            }
        }
        stage ('Build Docker Image') {
            steps {
                script {
                    sh '''
                        docker build -t ${DOCKER_IMAGE_NAME}:{DOCKER_TAG} .
                }
            }
        }
    }
    post {
        success {
            echo 'Divine, you have tried.'
        }
    }
}