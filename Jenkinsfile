pipeline {

    agent any
    tools {
        maven 'maven_3_6_0'
    }

    stages {
        stage('Testing Stage') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Install Stage') {
            steps {
                sh 'mvn install'
            }
        }
        stage('Compile Stage') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Package Stage') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}