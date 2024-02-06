pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        packageVersion = ''
    }
    options {
        ansiColor('xterm')
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds() 
    }
    // build
    stages {
        stage('Get the Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version is: ${packageVersion}"
                }
            }
        }
        stage('Install dependencies') {
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Build') {
            steps {
                sh """
                    ls -la
                    zip -r catalogue.zip ./* -x ".git" -x "*.zip"
                    ls -ltr
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                    echo "hello shellscript"
                    #sleep 10
                """
            }
        }
    }
    // post build
    post {
        always {
           echo "always run till pipeline runs " 
           deleteDir()
        }
        failure {
            echo "pipeline is failed"
        }
        success {
            echo "pipeline is success"
        }
    }
}