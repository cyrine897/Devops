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

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube Analysis"
                script {
                    def scannerHome = tool 'SonarScanner' // Ensure this name is correct
                    
                    // Use withCredentials to retrieve the token securely
                    withCredentials([string(credentialsId: 'sonarqueb-token', variable: 'SONAR_TOKEN')]) {
                        // Use bash explicitly
                        sh """
                            ${scannerHome}/bin/sonar-scanner -X \
                            -Dsonar.projectKey=devops-key \
                            -Dsonar.sources=src \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
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
