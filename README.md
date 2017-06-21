# K8S 

## Create TPR

```
kubectl create -f resource.yaml
```

## Create new Integration Flow

```
kubectl create -f ./flow.yaml
```

## Check it's there

```
kubectl get integrationflow -o json
```

## Basic concepts

How intgration flow is reflected in K8S?

### Scheduled flows

Let's take an example of "Timer to Node", integration flow that is to be executed every minute

### Cron job

A new cron job is created, something like:
```
apiVersion: batch/v2beta
kind: CronJob
metadata:
  name: Timer to Node
  taskID: 123456
  userID: 123456
  orgID: 123456
  tenantID: 123456
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```
