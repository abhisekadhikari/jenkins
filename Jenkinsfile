pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
    }

    environment {
        API_TOKEN = credentials('MY_SECRET_KEY')
    }

    stages {
        stage('Use Secret') {
            steps {
                echo "Starting build..."
                // Jenkins is smart: it will mask the secret in the logs!
                sh "echo 'The secret token is: \$API_TOKEN'"
            }
        }

        stage('Hello') {
            steps{
                echo 'Hello from GitHub to the jenkins!'
            }
        }

        stage("Test") {
            steps{
                echo "all the tests are passed"
            }
        }

        stage("Building") {
            steps{
                echo("Building...")
                sh "echo 'Build Version: ${env.BUILD_ID}' > version.txt"
            }
        }

        stage("Proceeding to deployment") {
            steps{
                echo("Deployment Successful")
            }
        }

        stage("Archive") {
            steps{
                // This tells Jenkins to "save" the file so you can download it later
                archiveArtifacts artifacts: 'version.txt', fingerprint: true
            }
        }
    }

    post{
        always {
            echo("Cleaning the workspace...")
            cleanWs()
        }

        success {
            slackSend(
                baseUrl: 'https://hooks.slack.com/services/',
                token: 'T0AH951CLPK/B0AGW7N257C/7l17JdCaQTinPmFSr7etgLgM', // The rest of your URL after /services/
                channel: '#all-cicd',
                color: 'good',
                message: "Test from Jenkins!"
            )
        }
        
        failure {
            slackSend(
                channel: '#all-cicd',
                color: 'danger',
                message: "🚨 Build FAILED: ${env.JOB_NAME} [${env.BUILD_NUMBER}] (${env.BUILD_URL})"
            )
        }
    }
}