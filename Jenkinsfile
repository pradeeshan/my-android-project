pipeline {
    agent any

    environment {
        ANDROID_HOME = "/Users/pradeeshan.n/Library/Android/sdk"
        PATH = "$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:/usr/local/bin"
        LC_ALL = "en_US.UTF-8"
        LANG = "en_US.UTF-8"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Identify Tag') {
            steps {
                script {
                    // This gets the tag name and sets it as the Build Name in Jenkins
                    def tag = sh(script: 'git describe --tags --exact-match', returnStdout: true).trim()
                    currentBuild.displayName = "${tag}"
                    echo "Now building version: ${tag}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'bundle install'
            }
        }

        stage('Fastlane Build') {
            steps {
                sh 'bundle exec fastlane android build_aab'
            }
        }
    }

    post {
        success {
            echo "Build complete for tag!"
            archiveArtifacts artifacts: 'app/build/outputs/bundle/release/*.aab'
        }
        failure {
            echo "Build failed. Check the Jenkins console logs."
        }
    }
}