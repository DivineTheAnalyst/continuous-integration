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
        stage ('Compile') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }
        stage ('Sonarqube Analysis') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'bfdd9c17-1e89-4513-bffa-a66a18d8dcef', variable: 'SONARQUBE_TOKEN')]){
                    sh '''
                        '${MAVEN_HOME}/bin/mvn' sonar:sonar \
                        -Dsonar.projectKey=calculator-jenkins \
                        -Dsonar.host.url=http://localhost:9000
                        -Dsonar.login=${SONARQUBE_TOKEN}
                    '''
                    }
                }
            }
        }
    }
}