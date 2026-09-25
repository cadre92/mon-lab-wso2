pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo '=== 1. Récupération du code depuis Git ==='
                checkout scm
            }
        }

        stage('Deploy to WSO2 MI') {
            steps {
                echo '=== 2. Déploiement des artefacts dans WSO2 MI ==='
                script {
                    // Copie automatique des API et Data Services vers le volume partagé WSO2
                    sh 'cp -r src/main/wso2mi/artifacts/* /wso2-deploy/'
                }
            }
        }

        stage('Health Check & Verification') {
            steps {
                echo '=== 3. Vérification des services ==='
                // Test simple d'appel à l'API HelloAPI
                sh 'curl -s http://wso2mi:8290/hello || true'
            }
        }
    }

    post {
        success {
            echo '=== Pipeline exécuté avec succès ! ==='
        }
        failure {
            echo '=== Erreur lors du déploiement ==='
        }
    }
}