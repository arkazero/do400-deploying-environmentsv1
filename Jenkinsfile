pipeline {
    agent {
        node {
            label 'maven'
        }
    }

    environment {
        OPENSHIFT_CLUSTER_DEV_URL = 'https://console-openshift-console.apps.na46a.prod.ole.redhat.com'
        OPENSHIFT_CLUSTER_DEV_API = 'https://api.na46a.prod.ole.redhat.com:6443'

        OPENSHIFT_PROJECT = ''
    }

    stages {
        stage('Tests') {
            steps {
                sh './mvnw clean test'
            }
        }

        stage('Login OC') {
            steps {
                sh '''
                    oc login -u hfkcyb -p e5e921e582ab4d298a34 https://api.na46a.prod.ole.redhat.com:6443
                    oc project hfkcyb-prueba 
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    ./mvnw clean package -Dquarkus.kubernetes.deploy=true
                '''
            }
        }
        stage('Logout OC') {
            steps {
                sh '''
                    oc logout
                '''
            }
        }
    }
}