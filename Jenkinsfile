pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
        timestamps()
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
                sh 'ls -R'
            }
        }

        stage('Lint Ansible') {
            steps {
                dir('ansible') {
                    sh 'ansible-playbook -i inventory.ini site.yml --syntax-check'
                }
            }
        }

        stage('Dry Run') {
            steps {
                dir('ansible') {
                    sh 'ansible-playbook -i inventory.ini site.yml --check'
                }
            }
        }

        stage('Approval') {
            steps {
                input message: 'Valider le déploiement ?', ok: 'Déployer'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }

            steps {
                dir('ansible') {
                    sh 'ansible-playbook -i inventory.ini site.yml'
                }
            }
        }

        stage('Smoke Test') {
            steps {
                sh '''
                    curl http://target1:5000/health
                    curl http://target2:5000/health
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
