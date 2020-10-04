pipeline {
    agent any

    environment{
        DOCKER_IMAGE_NAME = "baobk/train-schedule"
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

                    app = docker.build(DOCKER_IMAGE_NAME,"./docker")
                    app.inside {
                        sh 'make docker image'
                    }
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
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
           }
        }
    }
}