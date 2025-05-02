# Solutions for task in module 18:

#Task 1

Login to your cluster and create a pod with the image name as registry.k8s.io/busybox
    use the below command for the container touch /tmp/healthy; sleep 30; rm -f /tmp/healthy; sleep 600
    create a livenessprobe that executes the command cat /tmp/healthy after every 5 seconds, the first check should be after 5 second

    apiVersion: v1
    kind: Pod
    metadata:
      name: liveness-exec
      labels:
        test: liveness
    spec:
      containers:
      - name: liveness
        image: registry.k8s.io/busybox
        args:
        - /bin/sh
        - -c
        - touch /tmp/healthy; sleep 30; rm -f /tmp/healthy; sleep 600
        livenessProbe:
          exec:
            command:
            - cat
            - /tmp/healthy
        initialDelaySeconds: 5
        periodSeconds: 5

create another pod with the image name as registry.k8s.io/e2e-test-images/agnhost:2.40
    add the liveness and readiness probes that perform health checks on port 8080 on the path /healthz , the checks should start after 5 seconds for every 10 seconds
    
    apiVersion: v1
    kind: Pod
    metadata:
      name: liveness-http
      labels:
        test: liveness
    spec:
      containers:
      - name: liveness
        image: registry.k8s.io/e2e-test-images/agnhost:2.40
        args:
        - liveness
        readinessProbe:
          httpGet:
            port: 8080
            path: /healthz
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          httpGet:
            port: 8080
            path: /healthz
          initialDelaySeconds: 5
          periodSeconds: 10





