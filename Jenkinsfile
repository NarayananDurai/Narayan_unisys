pipeline {
    agent any
    stages {
        // create stage 1 for fetching git repo info
        stage('fetching git repo details'){
            // steps
            steps {
                echo 'fetching git repo'
                git branch: 'springboot', url:'https://github.com/NarayananDurai/Narayan_unisys.git'
                sh 'ls'
            }
        }
        // creating second stage for doing SAST analysis for any Critical bugs
        stage('SAST using trivy for Critical vulns'){
            steps {
                echo 'using trivy to scan code pushed by developer'
                sh 'trivy  fs --scanners  vuln,secret,misconfig  .'
            }
        }
        stage ('compose for build'){
            steps {
                echo 'docker-compose running'
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
                sh 'docker-compose ps'
            }
        }

        // using docker pipeline to build and push 
        stage('building image and pushing it') {
            steps {
                echo 'using docker pipeline plugin to build and push image'
                script {
                    def imageName = "narann/narayanc5"
                    def imageTag  = "appversion$BUILD_NUMBER"
                    def naraCred = "b58af617-86ca-485d-b33d-7ce908a1ae9c"
                    // building image 
                    docker.build(imageName + ":" + imageTag , " -f Dockerfile .")
                    // pushing image 
                    docker.withRegistry('https://registry.hub.docker.com',naraCred){
                        docker.image(imageName + ":" + imageTag).push()
                    }
                }
            }            
        }
        stage ('check connection with kubectl to AKS'){
            steps{
                sh 'kubectl version'
                sh 'kubectl get nodes'
            }
        }
    }
}