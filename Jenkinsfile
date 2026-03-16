pipeline {
    agent any

    environment {
        DEV_CLUSTER = "dev"
        STAGE_CLUSTER = "stage"
        PROD_CLUSTER = "prod"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/org/roboshop-monitoring.git'
            }
        }

        stage('Deploy Prometheus DEV') {
            steps {
                sh '''
                kubectl config use-context ${DEV_CLUSTER}

                helm upgrade --install prometheus \
                prometheus-community/prometheus \
                -n monitoring \
                --create-namespace \
                -f prometheus/dev-values.yaml
                '''
            }
        }

        stage('Deploy Prometheus STAGE') {
            steps {
                sh '''
                kubectl config use-context ${STAGE_CLUSTER}

                helm upgrade --install prometheus \
                prometheus-community/prometheus \
                -n monitoring \
                -f prometheus/stage-values.yaml
                '''
            }
        }

        stage('Deploy Prometheus PROD') {
            steps {
                sh '''
                kubectl config use-context ${PROD_CLUSTER}

                helm upgrade --install prometheus \
                prometheus-community/prometheus \
                -n monitoring \
                -f prometheus/prod-values.yaml
                '''
            }
        }

        stage('Deploy Central Grafana') {
            steps {
                sh '''
                kubectl config use-context ${PROD_CLUSTER}

                helm upgrade --install grafana \
                grafana/grafana \
                -n monitoring \
                -f grafana/values.yaml
                '''
            }
        }
    }
}