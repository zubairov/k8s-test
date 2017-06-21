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
```yaml
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
  concurrencyPolicy: Forbid
  successfulJobsHistoryLimit: 100
  failedJobsHistoryLimit: 100
  startingDeadlineSeconds: 10
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

Notes
 * ``startingDeadlineSeconds`` should be 10% of the scheduled interval, e.g. 6 seconds for every minute or 6 minutes for every hour

