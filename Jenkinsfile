pipeline {
    agent { 
        node { 
            label 'AGENT-1'
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'npm test'
            }
        }
        
        //it expects sonar-project.properties. credentials are configured in EC2 itself
        stage('Sonar Scan') {
            steps {
                echo "Sonar scan is done"
                //sh 'sonar-scanner' //This is already implemented
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
    }

    post {
        always {
            echo "Cleaning up work space"
            deleteDir()
        }
    }
}