pipeline {
    agent any

    environment {
        // Updated to your specific user path from the logs
        ANDROID_HOME = "/Users/pradeeshan.n/Library/Android/sdk"
        PATH = "$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:/usr/local/bin"
        
        // Fixes the UTF-8 warning in your logs
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