# AKS monolith deployment scripts

This folder containing scripts and Kubernetes resources configurations to run ThingsBoard in Monolith mode on Azure AKS cluster.

You can find the deployment guide by the [**link**](https://thingsboard.io/docs/user-guide/install/cluster/azure-monolith-setup/)

#clone
git clone https://github.com/pabrojast/thingsboard-ce-k8s.git
cd thingsboard-ce-k8s/azure/monolith

#env
export AKS_LOCATION=xxx
export AKS_RESOURCE_GROUP=xxx
export POSTGRESS_USER=xxx
export POSTGRESS_PASS=xxx
export TB_DATABASE_NAME=xxx

#setup db
az postgres flexible-server create --location $AKS_LOCATION --resource-group $AKS_RESOURCE_GROUP --name $TB_DATABASE_NAME --admin-user $POSTGRESS_USER --admin-password $POSTGRESS_PASS --public-access 0.0.0.0 --storage-size 32 --version 12 -d thingsboard --sku-name Standard_B1ms --tier GeneralPurpose

#install
 ./k8s-install-tb.sh --loadDemo

#deploy
./k8s-deploy-resources.sh
 
#https
kubectl apply -f receipts/https-load-balancer.yml

#mqtt
kubectl apply -f receipts/mqtt-load-balancer.yml

#kubectl get service
NAME                      TYPE           CLUSTER-IP     EXTERNAL-IP           PORT(S)                         AGE
tb-mqtt-loadbalancer      LoadBalancer   10.0.143.217   xxxxxxxxxxxxxx       1883:30504/TCP,8883:31408/TCP   11m
