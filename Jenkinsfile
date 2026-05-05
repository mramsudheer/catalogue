pipeline{
    agent{
        node{
            label 'roboshop'
        }
    }
    environment{
        appVersion =""
        ACC_ID = "424848769611"
        region = "us-east-1"
    }
    options{
        timeout(time: 5, unit:'MINUTES' )
    }
    stages{
        stage('Read version'){
            steps{
                /*script{
                    // Load and parse the JSON file
                    def packageJson = readJSON file: 'package.json'

                    // Access the Fields Directly
                    appVersion = packageJson.version
                    echo "Building version ${appVersion}"
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
        stage('Install Dependencies'){
            steps{
                script{
                    sh """
                        npm install
                    """
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    sh """
                        docker build -t catalogue:${appVersion} .
                    """
                }
            }
        }
    }
}