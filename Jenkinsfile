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
//        stage ('Quality Gate'){
//            steps {
//                sleep(10)
//                timeout(time: 1, unit: 'MINUTES') {
//                    waitForQualityGate abortPipeline: true                    
//               }
//           }
//       }
        stage ('Deploy Backend') {
           steps {
               deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
           } 
        }
        stage ('API Test') {
           steps {
               git credentialsId: 'LoginGithub', url: 'https://github.com/JilvanJr/tasks-api-test'
               bat 'mvn test'
           } 
        }
    }
}