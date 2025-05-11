# Example Voting App

A simple distributed application running across multiple Docker containers.

## Getting started
First git clone :
![git clone](https://github.com/user-attachments/assets/43bcadc2-a5d6-4cb6-97ce-311582777728)

Up minikube :
![minikube start](https://github.com/user-attachments/assets/69102b9e-9299-4ec9-9469-5816913fa0cb)


## Run the app in Kubernetes

The folder k8s-specifications contains the YAML specifications of the Voting App's services.

Run the following command to create the deployments and services. Note it will create these resources in your current namespace (`default` if you haven't changed it.)

```shell
kubectl create -f k8s-specifications/
```
![architecture](https://github.com/user-attachments/assets/87f0ea7a-ec7c-4941-9ed5-fcf9f130ecbb)
![kubectl apply -f k8s-specifications](https://github.com/user-attachments/assets/36c0af5b-86d8-4b6d-94bb-60b7493d336f)

![demande ouverture du fichiers vote et service](https://github.com/user-attachments/assets/2b2f5f5f-e66f-439a-aae0-a8b43dd259de)



The `vote` web app is then available on port 31000 on each host of the cluster, the `result` web app is available on port 31001.
![vote](https://github.com/user-attachments/assets/82116a13-5002-4dc9-befb-d103cec558d5)
![result](https://github.com/user-attachments/assets/44704c59-20e0-4786-88ab-b85c196feec3)



To remove them, run:

```shell
kubectl delete -f k8s-specifications/
```

## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
