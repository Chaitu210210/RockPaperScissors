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
               git branch: 'main', credentialsId: '3d5f286e-1309-4b36-9d17-d7b337de1c6d', url: 'https://github.com/Chaitu210210/RockPaperScissors'
                script {
                   script {
                    sh 'sudo rm -rf /home/ubuntu/DevSecOps-main/*'
                    sh 'sudo ls -l /home/ubuntu/DevSecOps-main/'
                       
                    def sourceDir = "/var/lib/jenkins/workspace/Desecops_main"

                    // Destination directory
                    def destDir = "/home/ubuntu/DevSecOps-main"

                    // Copy all files from sourceDir to destDir
                    sh "sudo cp -r ${sourceDir}/* ${destDir}"
                }
                }
        }
        }     
        stage("Sonarqube Analysis") {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=One \
                    -Dsonar.projectKey=One'''
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
                    def directoryPath = '/var/www/New/html'

                    // Use sudo to delete all files in the directory
                    sh "sudo rm -rf ${directoryPath}/*"
                
                    // Source directory
                    def sourceDir = "/home/ubuntu/DevSecOps-main"

                    // Destination directory
                    def destDir = "/var/www/New/html"

                    // Copy all files from sourceDir to destDir
                    sh "sudo cp -r ${sourceDir}/* ${destDir}"
                    
                    sh 'sudo service nginx restart'
                }
            }
        }
    }
}
