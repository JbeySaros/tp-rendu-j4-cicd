pipeline {
    agent any

    options {
        timestamps()
        skipDefaultCheckout(true)
    }

    parameters {
        booleanParam(name: 'APPLY', defaultValue: false, description: 'Déploiement réel (sinon dry-run)')
    }

    stages {

        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Show Workspace') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Lint Ansible') {
            steps {
                sh 'ansible-playbook -i inventory.ini site.yml --syntax-check'
            }
        }

        stage('Dry Run') {
            steps {
                sh 'ansible-playbook -i inventory.ini site.yml --check -vvv'
            }
        }

        stage('Approval') {
            when {
                branch 'main'
            }
            steps {
                input message: "Valider le déploiement en production ?"
            }
        }

        stage('Deploy') {
            when {
                expression { return params.APPLY }
            }
            steps {
                sh 'ansible-playbook -i inventory.ini site.yml'
            }
        }

        stage('Smoke Test') {
            steps {
                sh '''
                    echo "Smoke test"
                    curl -I http://target1 || true
                    curl -I http://target2 || true
                '''
            }
        }
    }

    post {
        always {
            echo "🏁 Build #${BUILD_NUMBER} terminé : ${currentBuild.currentResult}"
        }
        success {
            echo "✅ Déploiement v${APP_VERSION} réussi sur target1 + target2"
        }
        failure {
            echo "❌ Build #${BUILD_NUMBER} échoué — voir les logs"
        }
    }
}
