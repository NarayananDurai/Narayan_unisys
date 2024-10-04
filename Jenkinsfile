pipeline {
    agent any

    stages {
        stage('fetching github code') {
            steps {
                // using echo to print message 
                echo 'we are now fetching github code in below'
                // using git to download git repo 
                git branch: 'main', url: 'https://github.com/NarayananDurai/Narayan_unisys.git'
                // using sh command to run some commands
                sh 'ls '
            }
        }
        // verify docker and trivy 
        stage('checking docker and compose status') {
            steps {
                echo 'checking docker status'
                sh 'docker version'
                echo 'checking docker compose status'
                sh 'docker-compose version'
                
            }
            
        }
        // using compsoe and curl 
        stage('compsoe for build and curl for app status test') {
            steps {
                echo 'running docker compose'
                sh 'cd docker-compose down'
                sh 'docker-compose up -d --build'
                sh 'docker-compose ps'
                sh 'curl -f http://localhost:3031'
            }
            
        }
        // using docker pipeline to build and push 
        stage('building image and pushing it') {
            steps {
                echo 'using docker pipeline plugin to build and push image'
                sh 'cp ./resources/webappss/flaskapp .'
                script {
                    def imageName = "narann/flaskday4"
                    def imageTag  = "appversion$BUILD_NUMBER"
                    def ashuCred = "b58af617-86ca-485d-b33d-7ce908a1ae9c"
                    // building image 
                    docker.build(imageName + ":" + imageTag , " -f Dockerfile .")
                    // pushing image 
                    docker.withRegistry('https://registry.hub.docker.com',ashuCred){
                        docker.image(imageName + ":" + imageTag).push()
                    }
                }
            }
            
        }
    }
}