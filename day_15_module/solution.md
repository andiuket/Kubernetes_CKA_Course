#S


#Task 1

create a pod with nginx as the image and add the nodeffinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype = ssd
    check the status of the pod and see why it is not scheduled
    add the label to your worker01 node as distype=ssd and then check the status of the pod
    It should be scheduled on worker node 1

