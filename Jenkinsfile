pipeline {
    agent any
    
    tools {
        maven "M39"
    }

    environment {
        REPO_URL = "https://github.com/saleenakanaka/mavenRepo.git"
        BRANCH = "master"
        APP_NAME = "app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'echo "Printing Maven version"'
                sh 'mvn -version'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Package JAR as TAR') {
            steps {
                script {
                    // Find generated jar (adjust if needed)
                    sh '''
                        JAR_FILE=$(ls target/*.jar | head -n 1)
                        mkdir -p release
                        cp $JAR_FILE release/
                        cd release
                        tar -cvf ${APP_NAME}.tar *.jar
                    '''
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'release/*.tar', fingerprint: true
            }
        }
    }
}
