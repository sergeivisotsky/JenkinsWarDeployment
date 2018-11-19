pipeline {

    agent any
    tools {
        maven 'maven_3_5_0'
    }

    stages {
        stage('Package Stage') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Install Stage') {
            steps {
                bat 'mvn install'
            }
        }
        stage('Compile Stage') {
            steps {
                bat 'mvn compile'
            }
        }
        stage('Testing Stage') {
            steps {
                bat 'mvn test'
            }
        }

        /*stage('Deployment Stage') {
            steps {
                mat 'mvn deploy'
            }
        }*/
    }
}