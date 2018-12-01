pipeline {

    agent any
    tools {
        maven 'maven_3_5_0'
    }

    env.UNIX = isUnix()

    stages {
        stage('Testing Stage') {
            steps {
                if(Boolean.valueOf(env.UNIX)) {
                    sh 'mvn test'
                } else {
                    bat 'mvn test'
                }
            }
        }
        stage('Install Stage') {
            steps {
                if(Boolean.valueOf(env.UNIX)) {
                    sh 'mvn install'
                } else {
                    bat 'mvn install'
                }
            }
        }
        stage('Compile Stage') {
            steps {
                if(Boolean.valueOf(env.UNIX)) {
                    sh 'mvn compile'
                } else {
                    bat 'mvn compile'
                }
            }
        }
        stage('Package Stage') {
            steps {
                if(Boolean.valueOf(env.UNIX)) {
                    sh 'mvn clean package'
                } else {
                    bat 'mvn clean package'
                }
            }
        }
    }
}