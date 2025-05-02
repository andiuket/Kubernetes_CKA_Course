# Solutions for task in module 14:

f you do not already have a Kubernetes cluster, you can create a local Kubernetes cluster by following Day06 Video Also, could you do the port binding at the cluster level if you are using KIND? The Day9 video has the details on how to do that.
Task details

    Taint both of your worker nodes as below

    worker01--> gpu=true:NoSchedule , worker02--> gpu=false:NoSchedule

    #Task 1

    Steps:

        kubectl taint node cka-cluster-multin-worker gpu=true:NoSchedule

        kubectl taint node cka-cluster-multin-worker2 gpu=false:NoSchedule

    Create a new pod with the image nginx and see why it's not getting scheduled on worker nodes and control plane nodes.

    Steps:

        kubectl run nginx-pod --image=nginx port 80

        kubectl get pod/nginx-pod

           <!-- NAME        READY   STATUS    RESTARTS   AGE
           nginx-pod   0/1     Pending   0          69s -->
        
        kubectl describe pod/nginx-pod

            <!-- Warning  FailedScheduling  2m20s  default-scheduler  0/3 nodes are available: 1 node(s) had untolerated taint {gpu: false}, 1 node(s) had untolerated taint {gpu: true}, 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }. preemption: 0/3 nodes are available: 3 Preemption is not helpful for scheduling. -->

    #Task 2

    <!-- Create a toleration on the pod gpu=true:NoSchedule to match with the taint on worker01 -->

    <!-- nginx-pod.yaml  -->

        apiVersion: v1
        kind: Pod
        metadata:
        name: nginx-pod
        spec:
        containers:
        - name: nginx-container
            image: nginx
        tolerations:
        - key: "gpu"
            operator: "Equal"
            effect: "NoSchedule"
            value: "true"

        kubeclt apply -f nginx-pod.yaml

        <!-- NAME        READY   STATUS    RESTARTS   AGE     IP   NODE    NOMINATED NODE   READINESS GATES
        nginx-pod   1/1     Running   0          3m33s   10.244.2.18  cka-cluster-multin-worker   <none> -->

    #Task 3

    <!-- Create a new pod with the image redis , it should be scheduled on control plane node

    Add the taint back on the control plane node(the one that was removed) -->

        kubectl taint nodes cka-cluster-multin-control-plane node-role.kubernetes.io/control-plane:NoSchedule-

    <!-- redis-pod.yaml -->
       
        apiVersion: v1
        kind: Pod
        metadata:
        name: redis-pod
        spec:
        containers:
        - name: reids-container
            image: redis

        kubectl apply -f redis-pod.yaml 

        kubectl apply -f redis-pod.yaml 

        <!-- NAME        READY   STATUS    RESTARTS   AGE
        redis-pod   1/1     Running   0          2m36s -->

        kubectl taint nodes cka-cluster-multin-control-plane node-role.kubernetes.io/control-plane:NoSchedule