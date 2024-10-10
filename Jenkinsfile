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
    }
}