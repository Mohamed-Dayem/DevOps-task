1- I used a simple html file as web code 

2- using an Apache base image I wrote a dockerfile to containerize the app
![Screenshot 2024-03-23 143047](https://github.com/Mohamed-Dayem/DevOps-task/assets/141914187/eb7667aa-8fc6-4062-986d-e77b97b1e616)

3- using jenkins I configured a pipeline to automate the process

the first step
![Screenshot 2024-03-23 143722](https://github.com/Mohamed-Dayem/DevOps-task/assets/141914187/c92ae9cc-57ea-4855-ae0f-b6afc1ee240d)
at the build step, I create credentials in Jenkins for my Docker Hub Repo to push my image

the second step
![ddd](https://github.com/Mohamed-Dayem/DevOps-task/assets/141914187/7b98eb6e-a7b9-4a5c-9f33-666da6fe0dac)
at the deployment step, I stored the kubeconfig file for my Kubernetes cluster as a secret to access the cluster and deploy the Kubernetes Manifest 
I used the envsubst command to substitute the environment variable $(build_Number) in the deployment.yaml file

4- For the Deployment i used AWS EKS to create a Kubernetes Cluster then i deployed the following
  - Deployment:
    ![image](https://github.com/Mohamed-Dayem/DevOps-task/assets/141914187/dd5b3a5d-03e2-4cbf-8cae-7ce3f7b25773)
    I set the num of replicas to 1
     Then I installed the metrics server and created a Horizontal Pod Autoscaler resource to achieve the scalability of the application

            $$   kubectl autoscale deployment demo-deploy --cpu-percent=50 --min=1 --max=10   $$
    - Nodeport Service:
      ![image](https://github.com/Mohamed-Dayem/DevOps-task/assets/141914187/1b0c993b-df6f-4dc8-ae77-c1772e7be302)
       The Node port Service allows the pods to connect to the internet and be accessible
       
