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
        
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Ajouter ici les commandes de déploiement nécessaires
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
