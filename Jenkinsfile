pipeline {
    agent any

    stages {
        stage('GIT Checkout') {
            steps {
                echo "Getting Project from Git"
                git branch: 'master', credentialsId: '20', url: 'https://github.com/cyrine897/Devops.git'
            }
        }

        stage('MVN CLEAN') {
            steps {
                echo "Running Maven Clean"
                sh 'mvn clean'
            }
        }

        stage('MVN COMPILE') {
            steps {
                echo "Running Maven Compile"
                sh 'mvn compile'
            }
        }

        stage('Sonarqube') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=22922325Rr24.'
            }
        }
                stage('Deploy to Nexus') {
            steps {
                sh 'mvn deploy'
            }
        }



        stage('Quality Gate') {
            steps {
                echo "Checking SonarQube Quality Gate"
                waitForQualityGate abortPipeline: true
            }
        }
    }

    // Optional: Cleanup stage
    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
    }
}
