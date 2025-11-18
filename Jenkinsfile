pipeline {
    agent {
         node {
          label 'udh-1'
       }
    }
    
    environment {
        DEPLOY_NAME = 'nginx-dep'
        Name_inf   = 'k8s_servise'
        URL_inf    = 'git@github.com:ABVstudio/k8s_servise.git'
        REGION     = 'us-east-1'
        CLUSTER    = 'lepseyname_cluster'
    }

    stages {
        stage('download') {
            steps {
                sh "if [ -d ${Name_inf} ]; then cd ${Name_inf} && git pull ;else git clone ${URL_inf}; fi"
            }
        }
        stage('kubectl update config') {
            steps {
                sh "aws eks update-kubeconfig --region ${REGION} --name ${CLUSTER}"
            }
        }
        stage('apply servise') {
            steps {
                sh "kubectl apply -f ${Name_inf}/*.yaml"
            }
        }
        stage('update deployments') {
            steps {
                sh "kubectl rollout restart deployment ${DEPLOY_NAME}"
            }
        }
    }
}
