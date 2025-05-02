# Solutions for task in module 14:

f you do not already have a Kubernetes cluster, you can create a local Kubernetes cluster by following Day06 Video Also, could you do the port binding at the cluster level if you are using KIND? The Day9 video has the details on how to do that.
Task details

    Taint both of your worker nodes as below

    worker01--> gpu=true:NoSchedule , worker02--> gpu=false:NoSchedule

    #Task 1

        kubectl taint node cka-cluster-multin-worker gpu=true:NoSchedule

        kubectl taint node cka-cluster-multin-worker2 gpu=false:NoSchedule

    Create a new pod with the image nginx and see why it's not getting scheduled on worker nodes and control plane nodes.

        kubectl run nginx-pod --image=nginx port 80

        kubectl get pod/nginx-pod

           NAME        READY   STATUS    RESTARTS   AGE
           nginx-pod   0/1     Pending   0          69s
        
        kubectl describe pod/nginx-pod

            Warning  FailedScheduling  2m20s  default-scheduler  0/3 nodes are available: 1 node(s) had untolerated taint {gpu: false}, 1 node(s) had untolerated taint {gpu: true}, 1 node(s) had untolerated taint {node-role.kubernetes.io/control-plane: }. preemption: 0/3 nodes are available: 3 Preemption is not helpful for scheduling.



