pipeline {
    agent any
    environment {
        //be sure to replace "bhavukm" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "rakeshharidas007/projecti"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/projecti.zip'
            }
        }
        stage('Install Docker on testserver') {
             steps {
                script {
                    sh "ansible-playbook -i ./home/edureka/myproject/hosts ./home/edureka/myproject/dockerinstall.yaml"
                    sh "echo 'WE ARE DEPLOYING'"
                    }
                }
            }
        
        stage('Build Docker Image') {
             steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        
        stage('Push Docker Image') {
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
        
