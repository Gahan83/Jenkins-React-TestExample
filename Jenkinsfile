pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
        BUILD_PATH = "${WORKSPACE}\\build"
        DEPLOY_SERVER = "ec2-44-220-164-163.compute-1.amazonaws.com"
        DEPLOY_USER = "Administrator"
        DEPLOY_PASS = "FhKc@5OoTbHptzV)XdPH4NNRjGQR7nV?"
        DEPLOY_PATH = "D:\\CI_CD\\test_ReactJS"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'git credentials id', url: 'sample repo url'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Windows Server') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Uat-Server', usernameVariable: 'DEPLOY_USER', passwordVariable: 'DEPLOY_PASS')]) {
                        sh """
                        net use \\\\${DEPLOY_SERVER}\\C\$ /user:${DEPLOY_USER} ${DEPLOY_PASS}
                        robocopy ${BUILD_PATH} \\\\${DEPLOY_SERVER}\\C\$\\inetpub\\wwwroot\\dummy-app /E /MIR /XD node_modules
                        net use \\\\${DEPLOY_SERVER}\\C\$ /delete
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
