pipeline {
    agent any

    stages {
        stage("Building") {
            steps {
                sh "ant -f Build.xml -v"
            }

        }
    }    
}