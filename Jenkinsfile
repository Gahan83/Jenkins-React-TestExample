pipeline {
    agent any

    environment {
        IIS_HOST = 'ec2-44-220-164-163.compute-1.amazonaws.com'
        IIS_USER = 'Administrator'
        IIS_PASS = 'FhKc@5OoTbHptzV)XdPH4NNRjGQR7nV?'
        REMOTE_DIR = 'C:\\inetpub\\wwwroot\\ReactApp'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'npm run build'
                }
            }
        }

        stage('Publish to IIS') {
            steps {
                script {
                    // Compress the build folder
                    sh 'zip -r build.zip build/'

                    // Copy build.zip to IIS server via SCP
                    sh """
                    sshpass -p '${IIS_PASS}' scp -o StrictHostKeyChecking=no build.zip ${IIS_USER}@${IIS_HOST}:/C:/inetpub/wwwroot/
                    """

                    // Unzip and deploy on IIS server
                    sh """
                    sshpass -p '${IIS_PASS}' ssh -o StrictHostKeyChecking=no ${IIS_USER}@${IIS_HOST} powershell -Command "
                        Expand-Archive -Force C:\\inetpub\\wwwroot\\build.zip -DestinationPath ${REMOTE_DIR};
                        Remove-Item C:\\inetpub\\wwwroot\\build.zip
                    "
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
