pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    sh """
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker build -t  mahmeddayem/app:v${BUILD_NUMBER} .
                        docker push mahmeddayem/app:v${BUILD_NUMBER}
                    """
                }
            }
        }

        stage('Deployment') {
            steps {
                withCredentials([file(credentialsId: 'kube', variable: 'KUBECONFIG')]){
                    sh """
                        mv kube-manifist/Deployment.yaml kube-manifist/Deployment.yaml.tmp
                        cat kube-manifist/Deployment.yaml.tmp | envsubst > kube-manifist/Deployment.yaml
                        rm -rf kube-manifist/Deployment.yaml.tmp
                        kubectl apply -f kube-manifist --kubeconfig=${KUBECONFIG}
                    """
                }
            }
        }
    }
}
