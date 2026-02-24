pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
    }

    stages{
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
            echo 'Build and Cleanup successful!'
        }
    }
}