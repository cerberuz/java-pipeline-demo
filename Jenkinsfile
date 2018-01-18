pipeline {
    agent any

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