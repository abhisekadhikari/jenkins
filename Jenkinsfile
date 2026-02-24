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

    post {
        success {
            slackSend(
                tokenCredentialId: 'SLACK_WEBHOOK_URL',
                channel: '#all-cicd',
                color: 'good',
                message: "✅ *Build Success!* \n*Project:* ${env.JOB_NAME} \n*Build:* #${env.BUILD_NUMBER} \n*Link:* ${env.BUILD_URL}"
            )
        }
        failure {
            slackSend(
                tokenCredentialId: 'SLACK_WEBHOOK_URL',
                channel: '#all-cicd',
                color: 'danger',
                message: "🚨 *Build Failed!* \n*Project:* ${env.JOB_NAME} \n*Build:* #${env.BUILD_NUMBER} \n*Check Logs:* ${env.BUILD_URL}"
            )
        }
        always {
            echo "Cleaning the workspace..."
            cleanWs()
        }
    }
}