pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t fleetman-webapp:local .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh 'docker image inspect fleetman-webapp:local'
                echo 'Image Docker construite et vérifiée avec succès.'
            }
        }

        stage('Check Kubernetes Access') {
            steps {
                sh '''
                    kubectl \
                      --kubeconfig=/var/lib/jenkins/.kube/config \
                      get deployments -n fleetman
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès.'
        }
        failure {
            echo 'Le pipeline a échoué. Consulte les logs de la stage en erreur.'
        }
    }
}
