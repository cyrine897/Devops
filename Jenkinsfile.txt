pipeline {
    agent any 

    stages {
        stage('GIT Checkout') {
            steps {
                echo "Getting Project from Git"
                // Récupérer le code du dépôt Git
                git url: 'https://github.com/cyrine897/mon-projet-jenkins.git', branch: 'master'
            }
        }

        stage('MVN CLEAN') {
            steps {
                echo "Running Maven Clean"
                // Exécuter la commande Maven Clean
                sh 'mvn clean'
            }
        }

        stage('MVN COMPILE') {
            steps {
                echo "Running Maven Compile"
                // Exécuter la commande Maven Compile
                sh 'mvn compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube Analysis"
                script {
                    // Définir le chemin vers l'outil SonarQube Scanner
                    def scannerHome = tool 'SonarQubeScanner' // Assurez-vous que le nom correspond à votre configuration
                    // Exécuter l'analyse SonarQube
                    withSonarQubeEnv('SonarQube') { // Vérifiez le nom de la configuration SonarQube
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=devops-key -Dsonar.sources=src -Dsonar.host.url=http://localhost:9000"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo "Checking SonarQube Quality Gate"
                // Vérifier la qualité de la gate SonarQube
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
