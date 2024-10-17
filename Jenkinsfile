pipeline {
    agent any 
    stages {
        stage('GIT Checkout') {
            steps {
                echo "Getting Project from Git"
                git url: 'https://github.com/cyrine897/mon-projet-jenkins.git', branch: 'main'
            }
        }
        stage('MVN CLEAN') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('MVN COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }
    }
}
