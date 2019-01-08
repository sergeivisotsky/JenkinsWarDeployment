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
        stage('CI Build and push snapshot') {
            environment {
                PREVIEW_VERSION = "0.0.0-SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER"
                PREVIEW_NAMESPACE = "$APP_NAME-$BRANCH_NAME".toLowerCase()
                HELM_RELEASE = "$PREVIEW_NAMESPACE".toLowerCase()
            }

            steps {
                sh "mvn versions:set -DnewVersion=$PREVIEW_VERSION"
                sh "mvn install"
                sh 'export VERSION=$PREVIEW_VERSION'
            }
        }
        stage('Build Release') {
            when {
                branch 'master'
            }
            steps {
                // ensure we're not on a detached head
                sh "git checkout develop"
                sh "git config --global credential.helper store"

                sh "jx step git credentials"
                // so we can retrieve the version in later steps
                sh "echo \$(jx-release-version) > VERSION"
                sh "mvn versions:set -DnewVersion=\$(cat VERSION)"
                sh 'mvn clean verify'

                sh "git add --all"
                sh "git commit -m \"Release `cat VERSION`\" --allow-empty"
                sh "git tag -fa v\$(cat VERSION) -m \"Release version \$(cat VERSION)\""
                sh "git push origin v\$(cat VERSION)"
            }
            container('maven') {
                sh 'mvn clean deploy -DskipTests'

                sh 'export VERSION=`cat VERSION`'

                sh "git config --global credential.helper store"

                sh "jx step git credentials"
                sh "updatebot push-version --kind maven org.activiti:activiti-core-dependencies \$(cat VERSION) --merge false"
                sh "updatebot update --merge false"

            }
        }
    }
}