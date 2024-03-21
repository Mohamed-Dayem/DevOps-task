pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    sh """
                        sudo docker login -u ${USERNAME} -p ${PASSWORD}
                        sudo docker build -t  mahmeddayem/app:v${BUILD_NUMBER} .
                        sudo docker push mahmeddayem/app:v${BUILD_NUMBER}
                    """
                }
            }
        }

        stage('Deployment') {
            steps {
                withCredentials([file(credentialsId: 'kube', variable: 'KUBECONFIG')]){
                    sh """
                        mv kube-manifist/Deployment.yml kube-manifist/Deployment.yml.tmp
                        cat kube-manifist/Deployment.yml.tmp | envsubst > kube-manifist/Deployment.yml
                        rm -rf kube-manifist/Deployment.yml.tmp
                        kubectl apply -f kube-manifist --kubeconfig=${KUBECONFIG}
                    """
                }
            }
        }
    }
}
