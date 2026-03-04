pipeline {
    agent any // Tells Jenkins to allocate any available agent (executor and workspace)
    tools {
        maven "M39"
    }
    stages {
        stage('Build') {
            steps {
                script{
                    sh 'echo "print maven version"'
                    sh 'mvn -version'
                }
              }
        }
    }
}
