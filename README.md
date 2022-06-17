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

#Create a LoadBalancer service
kubectl create -f loadbalancer.yaml

#Delete a LoadBalancer service
kubectl create -f loadbalancer.yaml

#View all resources that exist in the eks-sample-app namespace.
kubectl get all -n eks-sample-app


#View the details of the deployed service
kubectl -n eks-sample-app describe service sample-service-loadbalancer


#remove the sample namespace, service, and deployment with the following command.
kubectl delete namespace eks-sample-app
