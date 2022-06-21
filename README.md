# wavesdev-eks
1->install aws cli
2-> aws configure 
3-> install kubectl
4-> install eksctl



# create cluster
eksctl create cluster --config-file cluster-1.22.yaml

# delete cluster
eksctl delete cluster --config-file cluster-1.22.yaml  --wait

# create node
eksctl create nodegroup --config-file eks-nodegroup.yaml
# delete node
eksctl delete nodegroup --config-file eks-nodegroup.yaml --wait

#install Installing the AWS Load Balancer Controller

reference :  https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html
#create iam oidc provider
eksctl utils associate-iam-oidc-provider --region=ap-southeast-1 --cluster=wavesdev-cluster --approve  
#create an iam policy
aws iam create-policy     --policy-name AWSLoadBalancerControllerIAMPolicy     --policy-document file://iam_policy.json
#create iam role and service account
eksctl create iamserviceaccount   --cluster=wavesdev-cluster   --namespace=kube-system   --name=aws-load-balancer-controller   --role-name "AmazonEKSLoadBalancerControllerRole"   --attach-policy-arn=arn:aws:iam::486306523629:policy/AWSLoadBalancerControllerIAMPolicy   --approve
#install target group binding CRDs
kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller/crds?ref=master"
#Install the AWS Load Balancer Controller using Helm V3 

#deploy helm chart
helm repo add eks https://aws.github.io/eks-charts
helm repo update
#configure AWS ALB to sit infront of ingress
helm install aws-load-balancer-controller eks/aws-load-balancer-controller   -n kube-system   --set clusterName=wavesdev-cluster   --set serviceAccount.create=false   --set serviceAccount.name=aws-load-balancer-controller
#Verify that the controller is installed.
kubectl get deployment -n kube-system aws-load-balancer-controller

#before this you need push docker image to aws ecr

# create namespace

kubectl create namespace eks-sample-app

#Apply the deployment manifest to your cluster.
kubectl apply -f eks-sample-deployment.yaml

# delete 

kubectl delete -f eks-sample-deployment.yaml


#Create a ClusterIP service

kubectl create -f clusterip.yaml
#Delete a ClusterIP service
kubectl delete -f clusterip.yaml

#Create the NodePort 
kubectl create -f nodeport.yaml

#Delete the NodePort 
kubectl delete -f nodeport.yaml


#Create a ingress
kubectl create -f ingress.yaml

#verify ingress
kubectl get ingress/eks-sample-ingress -n eks-sample-app

#Delete a ingress
kubectl create -f ingress.yaml
..................................................................

#View all resources that exist in the eks-sample-app namespace.
kubectl get all -n eks-sample-app


#View the details of the deployed service
kubectl -n eks-sample-app describe service sample-service-loadbalancer


#remove the sample namespace, service, and deployment with the following command.
kubectl delete namespace eks-sample-app



#important steps to deploy to eks

eksctl create cluster --config-file cluster-1.22.yaml
eksctl create nodegroup --config-file eks-nodegroup.yaml

#install Installing the AWS Load Balancer Controller
..........................................................................................................
reference :  https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html

#create iam oidc provider
eksctl utils associate-iam-oidc-provider --region=ap-southeast-1 --cluster=wavesdev-cluster --approve  
#create an iam policy
aws iam create-policy     --policy-name AWSLoadBalancerControllerIAMPolicy     --policy-document file://iam_policy.json
#create iam role and service account
eksctl create iamserviceaccount   --cluster=wavesdev-cluster   --namespace=kube-system   --name=aws-load-balancer-controller   --role-name "AmazonEKSLoadBalancerControllerRole"   --attach-policy-arn=arn:aws:iam::486306523629:policy/AWSLoadBalancerControllerIAMPolicy   --approve
#install target group binding CRDs
kubectl apply -k "github.com/aws/eks-charts/stable/aws-load-balancer-controller/crds?ref=master"
#Install the AWS Load Balancer Controller using Helm V3 

#deploy helm chart
helm repo add eks https://aws.github.io/eks-charts
helm repo update
#configure AWS ALB to sit infront of ingress
helm install aws-load-balancer-controller eks/aws-load-balancer-controller   -n kube-system   --set clusterName=wavesdev-cluster   --set serviceAccount.create=false   --set serviceAccount.name=aws-load-balancer-controller
#Verify that the controller is installed.
kubectl get deployment -n kube-system aws-load-balancer-controller
..............................................................................................................................
kubectl create namespace eks-sample-app
kubectl apply -f eks-sample-deployment.yaml
kubectl create -f nodeport.yaml
kubectl create -f ingress.yaml
