# Highly-available-web-server
BKBIET B-Tech final year project 

freestyle project jenkins 
#after cloning the source code from SCM(github)
export tag=nginx_server
export repository=460115791670.dkr.ecr.us-east-1.amazonaws.com/bkbiet-project

# login to AWS
aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY

# build Docker image using Dockerfile
sudo docker build -t $repository:$tag .

# login to Elastic Container Registry and docker login
aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin $repository

# push docker image to ECR
sudo docker push $repository:$tag

# login to kubernetes cluster (EKS)
aws eks --region us-east-1 update-kubeconfig  --name <eks_name>

# deploy the application
kubectl get nodes
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl rollout restart deploy my-deployment

# remove the dockerfile from server
sudo docker rmi $repository:$tag

# remove the aws credentials from the server
sudo rm -rf ~/.aws
