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
            archive 'dist/*.jar'
        }
    }   
}