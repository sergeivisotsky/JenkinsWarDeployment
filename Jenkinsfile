pipeline {

    agent any
    tools {
        maven 'maven_3_5_0'
    }

    stages {
        stage('Testing Stage') {
            steps {
                if(isUnix()) {
                    sh 'mvn test'
                } else {
                    bat 'mvn test'
                }
            }
        }
        stage('Install Stage') {
            steps {
                if(isUnix()) {
                    sh 'mvn install'
                } else {
                    bat 'mvn install'
                }
            }
        }
        stage('Compile Stage') {
            steps {
                if(isUnix()) {
                    sh 'mvn compile'
                } else {
                    bat 'mvn compile'
                }
            }
        }
        stage('Package Stage') {
            steps {
                if(isUnix()) {
                    sh 'mvn clean package'
                } else {
                    bat 'mvn clean package'
                }
            }
        }
    }
}