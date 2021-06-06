pipeline {
    agent any
    stages {
        stage ('Build Beckend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
    }
}