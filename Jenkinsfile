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
                withSonarQubeEnv('calculator-jenkins') {
                    sh '''
                        mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=calculator-jenkins \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=sqp_d25db8114c477fa23d1d04910fb1d62eb1c1c410
                    '''
                }
            }
        }
    }
}