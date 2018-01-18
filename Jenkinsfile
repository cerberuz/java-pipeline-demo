pipeline {
    agent none

    options {
        buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
    }

    stages {
        stage ('Unit Test') {
            agent {
                label 'apache'
            }
            steps {
                sh 'ant -f test.xml -v'
                junit 'reports/result.xml'
            }
        }
        stage("Build") {
            agent {
                label 'apache'
            }
            steps {
                sh "ant -f Build.xml -v"
            }
            post {
        success {
            archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
    } 

        }
        stage('Deploy') {
            agent {
                label 'apache'
            }
            steps {
                sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"
                sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/" 
            }
        }
        stage('Running on CentOS') {
            agent {
                label 'CentOS'
            }
            steps {
                sh "wget http://fperez1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
                sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
            }
        }
        stage('Running on Debian') {
            agent{
                docker 'openjdk:8u151-jre'
            }
            steps {
                sh "wget http://fperez1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
                sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
            }
        }
        stage('Promote To Green') {
            agent {
                label 'apache'
            }
            when {
                branch 'master'
            }
            steps {
                sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
            }
        }
        stage('Promote branch to master') {
            agent 'apache'
            when {
                branch 'development'
            }
            steps {
                echo "Stashing any local changes"
                sh "git stash"
                echo "Checking out development"
                sh "git checkout development"
                echo "checking out Master Branch"
                sh "git checkout master"
                echo "merging development into master branch"
                sh "git merge development"
                echo "pushing to origin master"
                sh "git push origin master"

            }
        }
    } 
      
}