pipeline{
    agent{
        node{
            label 'roboshop'
        }
    }
    environment{
        appVersion =""
    }
    options{
        timeout(time: 5, unit:'MINUTES' )
    }
    stages{
        stage('Read version'){
            steps{
                // Load and parse the JSON file
                def packageJson = readJSON file: 'package.json'

                // Access the Fields Directly
                appVersion = packageJson.version
                echo "Building version ${appVersion}"
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
                        docker build -t catalogue:${appVersion}
                    """
                }
            }
        }
    }
}