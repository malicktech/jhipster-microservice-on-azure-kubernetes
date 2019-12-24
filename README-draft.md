jhipster-microservice-on-aks

- [ ] https://dev.to/deepu105/create-full-microservice-stack-using-jhipster-domain-language-under-30-minutes-4ele

in root project dir 
create app.jdl
jhipster import-jdl app.jdl

Build docker image

./mvnw -ntp -Pprod verify jib:dockerBuild

fire everythings up

docker-compose up -d

stream the logs

docker-compose logs -f

- [ ] (Deploying JHipster Microservices on Azure Kubernetes Service (AKS))[https://dev.to/deepu105/deploying-jhipster-microservices-on-azure-kubernetes-service-aks-2g6d]

in root project dir 
jhipster import-jdl k8s.jdl

docker login
docker image tag store citizendiop/store & docker push citizendiop/store
docker image tag invoice citizendiop/invoice & docker push citizendiop/invoice
docker image tag notification citizendiop/notification & docker push citizendiop/notification

az aks get-credentials --resource-group eCommerceCluster --name eCommerceCluster
kubectl get nodes

* Deploying the application to AKS

run the below commands in the same order
* juste deploy registry and store (for ressoruce limitation)

cd kubernetes
kubectl apply -f registry
kubectl apply -f invoice
kubectl apply -f notification
kubectl apply -f store

wait untiil container running, status of container

    kubectl get pods -w

    

    kubectl get deployments

    kubectl get all

get the external IP for the application.

kubectl get service -o wide

or

    kubectl get service store

get registry url

    kubectl expose service jhipster-registry --type=LoadBalancer --name=exposed-registry
    kubectl get service exposed-registry

get pod logs

    kubectl logs -f pod_name

* debug
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/

    kubectl describe pods store-mysql-8-name

result : FailedScheduling, 0/2 nodes are available: 2 Insufficient cpu.

* mettre à l'échelle des noeuds - https://docs.microsoft.com/fr-fr/azure/aks/scale-cluster

az aks show --resource-group eCommerceCluster --name eCommerceCluster --query agentPoolProfiles
az aks scale --resource-group eCommerceCluster --name eCommerceCluster --node-count 4 --nodepool-name "nodepool1"

* so when create cluster, use 4 node

* delete resources

kubectl delete deployments notification
kubectl delete service notification-mongodb-0

or 

kubectl delete deployment.apps/notification-mongodb service/notification service/notification-mongodb
kubectl delete deployment.apps/invoice-mysql service/invoice-mysql service/invoice

# Notes

create app through jdl declaration file

# links

https://www.jhipster.tech/kubernetes/

https://github.com/jhipster/generator-jhipster/blob/master/rfcs/1-jhipster-rfc-k8s-operator.md



https://github.com/oktadeveloper/jhipster-microservices-example/blob/master/TUTORIAL.md