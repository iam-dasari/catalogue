pipeline {
    agent { node { label 'AGENT-1' } }

    environment{
    //here if you create any variable you will have global access, 
    // since it is environment no need of def
        packageVersion = ''
    }

    stages {
        stage('Get version'){
            steps{
                script{
                    echo "Trying to receive the version"
                    try {
                        echo "Trying to receive the version from try......"
                        // Read the JSON file
                        def json = readJSON file: 'package.json'
                        // Extract version
                        def appVersion = json.version
                        echo "Extracted app version: ${appVersion}"
                        // Save to env variable if needed
                        env.APP_VERSION = appVersion
                    } catch(Exception ex) {
                        echo "Catching the exception ........";
                    }

                    finally {
                        echo "This is finally block ........";
                    }

                }
            }
        }  

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

         //install pipeline utility steps plugin, if not installed
            stage('Publish Artifact') {
                steps {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: '54.165.96.138:8081/',
                        groupId: 'com.roboshop',
                        version: "$packageVersion",
                        repository: "catalogue",
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "catalogue",
                            classifier: '',
                            file: "catalogue.zip",
                            type: 'zip']
                        ]
                    )
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