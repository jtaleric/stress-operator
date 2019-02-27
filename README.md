# stress-operator

Basic operator to deploy load onto workers.

This operator uses [stress-ng](https://github.com/ColinIanKing/stress-ng) to generate a load against subsystems.

## stress-operator available options

```yaml
apiVersion: stress.example.com/v1alpha1
kind: Stress
metadata:
  name: example-stress
spec:
  size: 1	# Number of replicas
  cpu: 1	# start  N  workers  exercising  the  CPU  by  sequentially  working  through all the different CPU stress methods.
  mem: 1        # start N workers that copy 2MB of data from  a  shared  region  to  a  buffer  using memcpy(3)  and  then  move  the data in the buffer with memmove(3) with 3 different alignments.
  io: 1		# start  N workers continuously calling sync(2) to commit buffer cache to disk
```

Default timeout for the workload is 60s -- however, the workload is in a loop.

Example from the stress playbook
```yaml
  - name: stress
    image: "docker.io/polinux/stress-ng:latest"
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo Starting Stress : CPU={{ cpu }} MEM={{ mem }} IO={{ io }};stress-ng --cpu {{ cpu }} --memcpy {{ mem }} --io {{ io }} --timeout 60s --metrics-brief; done"]
```


