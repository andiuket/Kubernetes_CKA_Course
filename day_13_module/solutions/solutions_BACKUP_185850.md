# Solutions for task in module 13:

#Task 1

  Create a pod and try to schedule it manually without the scheduler.


<<<<<<< HEAD
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx-pod
      labels:
        app: v1
        tier: frontend
    spec:
       nodeName: kind-cka-cluster3-worker
       containers:
       - image: nginx
         name: nginx-container
         ports:
         - containerPort: 80
=======
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-pod
    labels:
      app: v1
      tier: frontend
  spec:
    nodeName: kind-cka-cluster3-worker
    containers:
    - image: nginx
      name: nginx-container
      ports:
      - containerPort: 80
>>>>>>> d7fc6ee99aaf5f62331d98d359d02439ea39cdcb

#Task 2

  Login to the control plane node and go to the directory of default static pod manifests and try to restart the control plane components

  step 1.

    docker exec -it kind-cka-cluster3-control-plane bash

  step 2. 
    
    cd /etc/kubernertes/manifests

  step 3
    
    system restart kubelet
    exit

  step 4.

    kubectl get pods -n kube-system


#Task 3

  Create 3 pods with the name as pod1, pod2 and pod3 based on the nginx image and use labels as env:test, env:dev and env:prod for each of these pods   respectively.


    Pod 1
 
    apiVersion: v1
    kind: Pod
    metadata:
      name: pod1
      labels:
        env: test
    spec:
      containers:
      - image: nginx
        name: nginx

    Pod 2

    apiVersion: v1
    kind: Pod
    metadata:
      name: pod2
      labels:
        env: dev
    spec:
      containers:
      - name: nginx
        image: nginx

    Pod 3

    apiVersion: v1
    kind: Pod
    metadata:
      name: pod3
      labels:
        env: prod
    spec:
      containers:
      - name: nginx
        image: nginx

#Task 4

    Then using the kubectl commands, filter the pods that have labels dev and prod.

    kubectl get pod --selector env=dev
    
    kubectl get pod --selector env=prov

   
   
             
