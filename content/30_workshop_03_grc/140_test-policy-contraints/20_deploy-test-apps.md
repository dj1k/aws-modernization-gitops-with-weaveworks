---
title: "Deploy test apps"
date: 2020-04-12T18:00:00-00:00
draft: false
weight: 20
---

Now that we havwe defined our contraint templates and deployed some contraints, let's see if they work!


### Test image repos in prod namespace


Earlier we define that all pods in the production namespace must only use images from `xxxx` repo. Let's deploy a pod using a different unauthorized repo.

Here's an example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: opa
  namespace: production
  labels:
    owner: me.agilebank.demo
spec:
  containers:
    - name: opa
      image: openpolicyagent/opa:0.9.2
      args:
        - "run"
        - "--server"
        - "--addr=localhost:8080"
      resources:
        limits:
          cpu: "100m"
          memory: "30Mi"
```

Let's add this to our git repo

```bash
mkdir example-apps

curl -o xxx example-apps/pod-unauthorized-repo.yaml

git add example-apps/pod-unauthorized-repo.yaml

git commit -m "adding pod to test allowed repos"

git push
```

Check what happened to the pod, run the following:

```bash

kubectl get pods opa -n production

```


