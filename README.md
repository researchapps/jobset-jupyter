# Jobset Jupyter

Reproducing issue in [https://github.com/kubernetes-sigs/jobset/issues/133](https://github.com/kubernetes-sigs/jobset/issues/133).

## Usage

Create a kind cluster

```bash
$ kind create cluster
```

Install JobSet:

```bash
$ kubectl apply --server-side -k github.com/kubernetes-sigs/jobset/config/default?ref=main
```

Apply the configs here!

```bash
$ kubectl apply -f job.yaml
```

This will create a cluster-level headless service:

```bash
$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
jupyter-rj   ClusterIP   None         <none>        <none>    39s
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4m9s
```

That we need to forward:

```bash
$ podname=$(kubectl get pods -o json | jq -r .items[].metadata.name)
$ kubectl port-forward ${podname} 8888
```

You can then open your browser to [http://localhost:8888](http://localhost:8888)
to enter your token (testing). When you are done, don't forget to cleanup!

```bash
$ kind delete cluster
```
