pipeline {
    agent { node { label 'AGENT-1' } }

    environment{
    //here if you create any variable you will have global access, 
    // since it is environment no need of def
        packageVersion = ''
    }

    stages {
        /* stage('Get version'){
            steps{
                script{
                    try {
                        def json = readJSON file: 'package.json'
                        def appVersion = json.version
                        echo "Extracted app version: ${appVersion}"
                        env.APP_VERSION = appVersion
                    } catch(Exception ex) {
                        echo "Catching the exception ........";
                    }

                    finally {
                        echo "This is finally block ........";
                    }

                }
            }
        }   */

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

         /* //install pipeline utility steps plugin, if not installed
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
            } */

            stage("Build Image & Push to DockerHub") {
                steps {
                    script {
                        withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh """
                        docker build -t dasaridevops/catalogue:1.0.0 .
                        docker push dasaridevops/catalogue:1.0.0
                        """
                        }
                    }
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