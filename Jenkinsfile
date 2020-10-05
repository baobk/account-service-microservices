pipeline {
    agent any

    environment{
        registry = "baobk/train-schedule"
        dockerImage =''
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
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"

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
                        dockerImage.push("latest")
                    }
                }
           }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}