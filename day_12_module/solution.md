apiVersion: apps/v1
kind:  DaemonSet
metadata:
  name: myapp-ds
  labels:
    tier: demo
spec:
  template:
    metadata:
      labels:
        tier: demo
      name: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
        - containerPort: 80
  selector:
    matchLabels:
      tier: demo
