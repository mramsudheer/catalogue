pipeline{
    agent{
        node{
            label 'roboshop'
        }
    }
    environment{
        appVersion = ""
        ACC_ID = "424848769611"
        region = "us-east-1"
    }
    options{
        timeout(time: 5, unit:'MINUTES' )
    }
    stages{
        stage('Read version'){
            steps{
                script{
                    // Load and parse the JSON file
                    def packageJson = readJSON file: 'package.json'

                    // Access the Fields Directly
                    appVersion = packageJson.version
                    echo "Building version ${appVersion}"
                }
            }
        }
        stage('Install Dependencies'){
            steps{
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('SonarQube Analysis'){
            steps{
                script{
                    def scannerHome = tool name: 'Sonar-8.0' //Agent configuration Name
                    withSonarQubeEnv('sonar-server') { // 'sonar-server' must match your Jenkins=>System Config name(this is for analysing and uploading to the server)
                        sh "${scannerHome}/bin/sonnar-scanner" // This commands reads sonar-project.properties file
                    }
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                /*script{
                    sh """
                        docker build -t catalogue:${appVersion} .
                    """
                }*/
                script{
                    withAWS(credentials: 'aws_credts', region: "${region}"){
                        // Commands here have AWS authentication
                        sh """
                            aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin  ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.${region}.amazonaws.com/roboshop/catalogue:${appVersion} .
                            docker push ${ACC_ID}.dkr.ecr.${region}.amazonaws.com/roboshop/catalogue:${appVersion}
                        """
                    }
                }
            }
        }
    }
    post {
        always{
            //Clean Workspace, Saves Disk Space, 

            //It ensures that the next time the job runs, 
            // it starts with a completely empty folder. 
            // This prevents old files from a previous run 
            // (like old compiled classes or cached config files) from interfering with the new build.

            //If your build handles sensitive data, certificates, or temporary credentials, 
            // cleanWs() ensures those files aren't left sitting on the build agent after 
            // the job is done.

            cleanWs() 
        }
    }
}