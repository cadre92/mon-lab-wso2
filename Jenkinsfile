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
                echo '=== Déploiement automatisé des artefacts WSO2 MI ==='
                script {
                    // 1. Déploiement des APIs XML
                    sh 'mkdir -p /wso2-deploy/repository/deployment/server/synapse-configs/default/api'
                    sh 'cp -r src/main/wso2mi/artifacts/apis/* /wso2-deploy/repository/deployment/server/synapse-configs/default/api/'

                    // 2. Déploiement des Data Services (.dbs)
                    sh 'mkdir -p /wso2-deploy/repository/deployment/server/synapse-configs/default/dataservices'
                    sh 'cp -r src/main/wso2mi/artifacts/data-services/* /wso2-deploy/repository/deployment/server/synapse-configs/default/dataservices/'
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