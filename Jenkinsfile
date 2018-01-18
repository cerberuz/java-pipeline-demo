pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
    }

    stages {
        stage("Build") {
            steps {
                sh "ant -f Build.xml -v"
            }

        }
    } 
    post {
        always {
            archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
    }   
}