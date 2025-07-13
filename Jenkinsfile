pipeline {
    agent { 
        node { 
            label 'AGENT-1'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pwd'
                sh 'ls -lrt'
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                echo "Unit testing is done here"
            }
        }
    }
}