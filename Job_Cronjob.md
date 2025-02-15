# Job

## Just Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: math-pod
spec:
  containers:
  - name: math-add
    image: ubuntu
    command: ["expr", "3", "+", "2"]
  restartPolicy: Never
```

## Sequentailly multiple job
```
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
sepc:
  completions: 3
  template:
    spec:
      containers:
      - name: math-add
        image: ubuntu
        command: ['expr', '3', '+', '2']
      restartPolicy: Never
```
- Can check the result as stdout
- When Job is created then Pod is also created
- And Job is deleted then Pod is also deleted


```
apiVersion: batch/v1
kind: Job
metadata:
  name: throw-dice-job
spec:
  backoffLimit: 15 # This is so the job does not quit before it succeeds.
  template:
    spec:
      containers:
      - name: throw-dice
        image: kodekloud/throw-dice
      restartPolicy: Never
```


```
$ k delete job math-add-job
```

## Parallelism

```
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
sepc:
  completions: 3
  parallelism: 3
  template:
    spec:
      containers:
      - name: math-add
        image: ubuntu
        command: ['expr', '3', '+', '2']
      restartPolicy: Never
```

# CronJobs
- More complex

```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: reporting-cron-job
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    sepc:
      completions: 3
      parallelism: 3
      template:
        spec:
          containers:
          - name: math-add
            image: ubuntu
            command: ['expr', '3', '+', '2']
          restartPolicy: Never

```
