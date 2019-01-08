pipeline {

    agent any
    tools {
        maven 'maven_3_6_0'
    }

    stages {

        stage('Compilation') {
            steps {
                echo '-=- compiling project -=-'
                sh 'mvn clean compile'
            }
        }
        stage('Testing') {
            steps {
                echo '-=- execute unit tests -=-'
                sh 'mvn test'
            }
        }
        stage('Installation') {
            steps {
                echo '-=- installing project -=-'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Packaging') {
            steps {
                echo '-=- packaging project -=-'
                sh 'mvn package -DskipTests'
            }
        }
        stage('Code inspection & quality gate') {
            steps {
                echo '-=- run code inspection & check quality gate -=-'
                sh 'mvn sonar:sonar'
            }
        }
    }
    stages {
        stage('CI Build and push snapshot') {
            environment {
                PREVIEW_VERSION = "0.0.0-SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER"
                PREVIEW_NAMESPACE = "$APP_NAME-$BRANCH_NAME".toLowerCase()
                HELM_RELEASE = "$PREVIEW_NAMESPACE".toLowerCase()
            }

            steps {
                container('maven') {
                    sh "mvn versions:set -DnewVersion=$PREVIEW_VERSION"
                    sh "mvn install"
                    sh 'export VERSION=$PREVIEW_VERSION'
                }
            }
        }
    }
}