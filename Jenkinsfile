pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo '📥 Clonage du dépôt...'
                checkout scm
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo '🔍 Analyse SonarQube...'
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo '📊 Analyse terminée'
        }
        success {
            echo '✅ Analyse réussie'
        }
        failure {
            echo '❌ Échec de l\'analyse'
        }
    }
}
