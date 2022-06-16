# wavesdev-eks
1->install aws cli
2-> aws configure 
3-> install kubectl
4-> install eksctl



# create cluster
eksctl create cluster --config-file cluster-1.22.yaml

# delete cluster
eksctl delete cluster --config-file cluster-1.22.yaml  --wait
