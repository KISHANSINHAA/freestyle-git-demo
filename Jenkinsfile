pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/KISHANSINHAA/freestyle-git-demo.git']]])
            }
        }
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/KISHANSINHAA/freestyle-git-demo.git'
                bat 'C:/Users/sinha/AppData/Local/Programs/Python/Python31/python.exe test.py'
            }
        }
        stage('Test') {
            steps {
                echo 'TESTING'
            }
        }
        stage('Docker Deploy') {
            steps {
                script {
                    echo 'Checking for existing container...'

                    // Check if container exists
                    def containerExists = bat(
                        script: 'docker ps -a --format "{{.Names}}" | findstr /C:"myapp_container11"',
                        returnStatus: true
                    )

                    if (containerExists == 0) {
                        // Container exists
                        def isRunning = bat(
                            script: 'docker ps --format "{{.Names}}" | findstr /C:"myapp_container11"',
                            returnStatus: true
                        )

                        if (isRunning == 0) {
                            echo 'Container is already running. Skipping deployment.'
                        } else {
                            echo 'Container exists but stopped. Starting it...'
                            bat 'docker start myapp_container11'
                        }

                    } else {
                        echo 'Container does not exist. Building and running new one...'
                        bat 'docker build -t myapp:latest .'
                        bat 'docker run -d --name myapp_container11 myapp:latest'
                    }

                    echo 'Docker deployment stage completed!'
                }
            }
        }
    }
}
