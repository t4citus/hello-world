pipeline {
    agent any
    tools {
        maven 'Maven-3'
        jdk 'jdk-17'
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
