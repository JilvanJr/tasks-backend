pipeline {
    agent any
    stages {
        stage ('Build Beckend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=8e87d439ab2f57c345c92cd9135820a7914ff680 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**src/test/**,**/model/**,**Application.java"
                }
            }
        }
        stage ('Quality Gate'){
            steps {
                sleep(40)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}