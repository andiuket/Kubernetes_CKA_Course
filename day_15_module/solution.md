#S


#Task 1

create a pod with nginx as the image and add the nodeffinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype = ssd
    check the status of the pod and see why it is not scheduled
    add the label to your worker01 node as distype=ssd and then check the status of the pod
    It should be scheduled on worker node 1

Step 1

    apiVersion: v1
    kind: Pod
    metadata:
    name: nginx-pod
    spec:
    containers:
    - image: nginx
        name: nginx-pod
    affinity:
        nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
            - key: disktype
                operator: In
                values:
                - ssd
    
    kubectl apply -f nginx-pod.yaml

    kubectl get pod/nginx-pod

    NAME        READY   STATUS    RESTARTS   AGE
    nginx-pod   0/1     Pending   0          38s

    kubectl label node cka-cluster-multin-worker

    NAME        READY   STATUS    RESTARTS   AGE
    nginx-pod   1/1     Running   0          8m16s


#Task 2

create a new pod with redis as the image and add the nodeaffinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype without any value
    add the label to worker02 node with disktype and no value
    ensure that pod2 should be scheduled on worker02 node

Steps: 

