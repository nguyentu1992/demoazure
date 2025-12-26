import groovy.json.JsonSlurper
pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout Repository') {
            steps {
            	deleteDir() // This deletes the workspace, removing any cached data
                // Use the checkout step with SCM
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/nguyentu1992/demoazure.git',
                        credentialsId: 'userpass_git' //
                    ]]
                ])
            }
        }

        stage('Build Services') {
            parallel {
                stage('Build Backend') {
                    steps {
                        script {
                            echo "Building Backend..."
                            sh "docker build -t backend ./backend"
                        }
                    }
                }
            }
        }

        stage('Run Services') {
            steps {
                script {
                    echo "Starting services with docker-compose..."
                    sh "docker-compose -f ${DOCKER_COMPOSE_FILE} up -d"
                }
            }
        }
    }
}
