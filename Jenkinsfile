pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'DEV', credentialsId: '3d5f286e-1309-4b36-9d17-d7b337de1c6d', url: 'https://github.com/Chaitu210210/RockPaperScissors.git'
            }
        }
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=One-1 \
                    -Dsonar.projectKey=One-1'''
                }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        stage('Deploying') {
            steps {
                script {
                    // Replace '/path/to/directory' with the actual path to the directory
                    def directoryPath = '/var/www/New-1/html'

                    // Use sudo to delete all files in the directory
                    sh "sudo rm -rf ${directoryPath}/*"
                    // Source directory
                    def sourceDir = "/home/ubuntu/project/DevSecOps-Project@2"

                    // Destination directory
                    def destDir = "/var/www/New-1/html"

                    // Copy all files from sourceDir to destDir
                    sh "sudo cp -r ${sourceDir}/* ${destDir}"
                }
            }
        }
    }
}
