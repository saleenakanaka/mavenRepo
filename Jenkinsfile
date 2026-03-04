pipeline {
    agent any
    
    tools {
        maven "M39"
    }

    environment {
        REPO_URL   = "https://github.com/saleenakanaka/mavenRepo.git"
        BRANCH     = "master"
        APP_NAME   = "app"
        DEPLOY_HOST = "localhost"
        DEPLOY_USER = "deploy"
        DEPLOY_PATH = "/Users/stephenraj/Downloads/learnings/deploy"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Package') {
            steps {
                sh '''
                    JAR_FILE=$(ls target/*.jar | head -n 1)
                    mkdir -p release
                    cp $JAR_FILE release/
                    cd release
                    tar -cvf ${APP_NAME}.tar *.jar
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'release/*.tar', fingerprint: true
            }
        }

        stage('Deploy to Server') {
            steps {
                
                    sh """
                        cp release/${APP_NAME}.tar ${DEPLOY_PATH}/
                            cd ${DEPLOY_PATH} &&
                            tar -xvf ${APP_NAME}.tar 
                        
                    """
                
            }
        }

        stage('Verify Deployment') {
            steps {
                sshagent(['ssh-credentials-id']) {
                    sh """
                        echo "App deployed"
                        
                        
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
