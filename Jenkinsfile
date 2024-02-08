pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        packageVersion = ''
        nexusURL = '3.228.9.255:8081'
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
                script {
                    sh """
                        ls -la
                        zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                        ls -ltr
                    """
                }
            }
        }
        stage('Publish Artifact') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusURL}",
                    groupId: 'com.roboshop',
                    version: "${packageVersion}",
                    repository: 'catalogue',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: 'catalogue',
                        classifier: '',
                        file: 'catalogue.zip',
                        type: 'zip']
                    ]
                )
            }
        }

        stage('Invoke_pipeline') {
             steps {
                script {
                    build(job: 'catalogue-deploy', parameters: [(name: 'version', value: ''), (name: 'environment', value: "")])
                }
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