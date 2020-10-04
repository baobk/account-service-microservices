pipeline {
    agent any

    environment{
        registry = "baobk/train-schedule"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh 'mvn clean install'
            }
        }

        stage('Build docker image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = dockerImage = docker.build registry + ":$BUILD_NUMBER"

                }
            }
        }
        stage('Push docker image') {
            when {
                branch 'master'
            }
           steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {

                        app.push("latest")
                    }
                }
           }
        }
    }
}