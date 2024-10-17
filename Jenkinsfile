pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning the Git repository...'
                git url: 'https://github.com/cyrine897/Devops.git'
            }
        }
        
        stage('Maven Build') {
            steps {
                echo 'Building the project with Maven...'
                sh 'mvn clean install'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube Analysis...'
                script {
                    // Ajoutez le scanner SonarQube avec les paramètres appropriés
                    withSonarQubeEnv('SonarQube') { // 'SonarQube' correspond à l'ID du serveur configuré
                        sh 'mvn sonar:sonar -Dsonar.projectKey=devops-key -Dsonar.host.url=http://localhost:9000'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Waiting for SonarQube Quality Gate...'
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
