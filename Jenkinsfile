pipeline {
    agent any
    tools {
        maven 'Maven-3'
    }
    stages {
        stage("Build") {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("Test") {
            steps {
                sh 'mvn test'
            }
        }
    }
}
