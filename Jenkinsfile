pipeline {
    agent any
    
    tools {
        // Utilisez exactement le même nom que dans Global Tool Configuration
        // Option 1: Si vous avez "Sonarqube Scanner"
        // 'hudson.plugins.sonar.SonarRunnerInstallation' 'Sonarqube Scanner'
        
        // Option 2: Si vous changez le nom vers "SonarQube Scanner"
        'hudson.plugins.sonar.SonarRunnerInstallation' 'sonar-scanner'
    }
    
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
                    // Utilisez le même nom que dans la section tools
                    def scannerHome = tool 'sonar-scanner'
                    
                    // Créez un fichier sonar-project.properties si il n'existe pas
                    writeFile file: 'sonar-project.properties', text: '''
sonar.projectKey=AppNesrine
sonar.projectName=AppNesrine
sonar.projectVersion=1.0.0
sonar.sources=.
sonar.sourceEncoding=UTF-8
sonar.exclusions=**/node_modules/**,**/*.log
'''
                    
                    // Exécutez l'analyse SonarQube
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    // Attendez le résultat de Quality Gate
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                echo '🚀 Déploiement...'
                // Ajoutez vos étapes de déploiement ici
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline terminé avec succès!'
        }
        failure {
            echo '❌ Échec du pipeline'
        }
        always {
            echo '📊 Analyse terminée'
        }
    }
}
