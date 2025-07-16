pipeline {
    agent any
    
    tools {
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
                    def scannerHome = tool 'sonar-scanner'
                    
                    // Vérifiez que le scanner est trouvé
                    echo "Scanner Home: ${scannerHome}"
                    sh "ls -la ${scannerHome}/bin/"
                    
                    // Créez le fichier de configuration avec plus de détails
                    writeFile file: 'sonar-project.properties', text: '''
sonar.projectKey=AppNesrine
sonar.projectName=AppNesrine
sonar.projectVersion=1.0.0
sonar.sources=.
sonar.sourceEncoding=UTF-8
sonar.exclusions=**/node_modules/**,**/*.log,**/sonar-scanner/**
sonar.verbose=true
'''
                    
                    // Affichez le contenu du fichier
                    sh "cat sonar-project.properties"
                    
                    // Testez la connexion avant l'analyse
                    withSonarQubeEnv('SonarQube') {
                        echo "SONAR_HOST_URL: ${env.SONAR_HOST_URL}"
                        echo "SONAR_AUTH_TOKEN: ${env.SONAR_AUTH_TOKEN ? 'Token présent' : 'Token manquant'}"
                        
                        // Exécutez avec plus de verbosité
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=AppNesrine \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=\${SONAR_HOST_URL} \
                                -Dsonar.login=\${SONAR_AUTH_TOKEN} \
                                -Dsonar.verbose=true
                        """
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
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
                sh 'echo "Déploiement simulé"'
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
