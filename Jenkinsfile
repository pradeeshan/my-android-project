pipeline {
    agent any

    environment {
        // Update these to match your server's environment
        ANDROID_HOME = "/Users/appadm/Library/Android/sdk"
        PATH = "$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:/usr/local/bin"
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

        stage('Install Dependencies') {
            steps {
                sh 'bundle install'
            }
        }

        stage('Fastlane Build') {
            steps {
                // This triggers the lane we wrote in the Fastfile
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