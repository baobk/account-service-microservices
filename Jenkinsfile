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
                    app = dockerImage = docker.build("baobk/train-schedule:${env.BUILD_NUMBER}")
                     app.inside {
                        sh 'echo Hello, World!'
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