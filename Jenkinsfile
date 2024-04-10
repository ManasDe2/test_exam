pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Test') {
            steps {
                script {
                    def output = sh(script: 'python3 test.py', returnStdout: true).trim()
                    println "Output of test.py: ${output}"
                    
                    if (output.contains('Passed')) {
                        currentBuild.result = 'SUCCESS'
                    } else {
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
        
        stage('Deployment') {
            when {
                expression {
                    // Only run the Deployment stage if the Test stage was successful
                    return currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    sh 'sudo -S cp -r ./*.html /var/www/html/'
                    sh 'sudo -S systemctl restart nginx'
                }
            }
        }
    }
}
