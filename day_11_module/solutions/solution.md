# Solution for the task in module 11
# A step by step rundown of my solution

Step 1.

Create a pod with two containers using a yaml file, one container for the app and the other an initcontainer with a CLI editor like vim.

apiVersion: v1
kind: pod
metadata:
  name: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', do echo service not running && sleep 3600]
    env:
    - name: FIRSTNAME
      value: "andrew"
  initContainers:
  - name: init-service
    image: busybox
    command: ['sh', '-c']
    args: ['until nslookup myservice.ns1.svc.cluster.local; do echo waiting for service to come up; sleep 2; done']

Step 2.

Save the file

Step 3.

Create the pod and confirm that pod is created and running.

kubectl apply -f <name of yaml>

kubectl get pod

Pod is created but one out of the two containers is running because the app container depends on the init-container to finish running before it can start, while the init-container is waiting for a service to initialized before it can finish running. So we create that service using the imperative way by first creating a deployment and then we create a new service by by using the expose command, or we can do it with a yaml.


Step 4.

kubectl create deploy nginx-deploy --image nginx --port 80 && kubectl get deploy/nginx-deploy

#To create a service in the ns1 namespace
kubectl apply -f myservice.yaml -n ns1

#To check that the pod is running as well as the two containers.
kubectl get pod/myapp
kubectl describe pod/myapp



















  
