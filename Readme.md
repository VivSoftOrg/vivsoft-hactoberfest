# This is VivSoft's Hactoberfest hackathon project. 

![Hactoberfest](https://img.evbuc.com/https%3A%2F%2Fcdn.evbuc.com%2Fimages%2F114217629%2F293389748072%2F1%2Foriginal.20201010-000608?w=1080&auto=format%2Ccompress&q=75&sharp=10&rect=1%2C197%2C3910%2C1955&s=359a3b37e04caa40a0420ddb5cbb8178)

We built a solution that uses Spire to attest Istio workload identities. The configuration allows identities to be tied not only to the nodes within the Kubernetes Cluster but also to the workloads (at the container image level) to ensure that there is a strong cryptographic identity for each container image running within the cluster. 

# Start with a local k3d cluster deployment on Mac

## Tools need to be installed before deployment
1. docker: `brew install docker`
2. kubectl: `brew install kubectl`
3. k3d : `brew install k3d`
4. makecert: `brew install mkcert`

## load images into local registry
1. create registry `bash boot.sh local_container_registry`
1. tag your image `docker image tag nginx:latest registry.localhost:5000/nginx:latest`
3. push images using `docker push registry.localhost:5000/nginx:latest`

## Deployment Options
1. Create Cluster `bash boot.sh local_up`
2. Deploy Istio `bash boot.sh istio`
3. Delete cluster without destroying registry `bash boot.sh local_down`
4. delete everything `bash boo.sh local_clear`

Note: 
1. k3d will handle loadbalancer so you dont need to worry about getting cluster IP address.
2. Add your apps virtual service hostname to `/etc/hosts` ie .. `127.0.0.1 myapp.local #virtual service`

## Deploy istio
1. Please create cluster before your run istio
2. Deploy command `bash boot.sh istio`

Note :
1. This will create self-signed certs and force traffic to go by https. You will see trusted certificated because it is generated by your machine and that cannot be trusted.
2. This will create a gateway and allow all traffic with `*.local`

## Deploy Spire Server/Agent:
1. You need to deploy k8s cluster and also need to have istio deployed in the previous step.
2. This script will deploy spire server and one agent per node.
3. You have to register node in spire server first then you can register workloads using parent SVID
4. To deploy spire agent and server ` bash boot.sh spire`

## SPIFEE workload registration and testing.
1. Here we will deploy a workload and then register.
2. you can deploy and register demo workload using `bash boot.sh spifee_workload`
