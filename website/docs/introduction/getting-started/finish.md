---
title: Other components
sidebar_position: 50
---

In this section we'll deploy the rest of the sample application efficiently using the power of Kustomize. The following kustomization file shows how you can reference other kustomizations and deploy multiple components together:

```file
../manifests/kustomization.yaml
```

:::tip
Notice that the catalog API is in this kustomization, didn't we already deploy it? 

Because Kubernetes uses a declarative mechanism we can apply the manifests for the catalog API again and expect that because all of the resources are already created Kubernetes will take no action.
:::

Run kustomization to deploy the rest of the components:

```bash
$ kubectl apply -k /workspace/manifests
```

You can use `kubectl wait` to make sure all the components have started before we proceed:

```bash
$ kubectl wait --for=condition=Ready --timeout=120s pods \
  -l app.kubernetes.io/created-by=eks-workshop -A
```

We'll now have a Namespace for each of our application components:

```bash
$ kubectl get namespaces -l app.kubernetes.io/created-by=eks-workshop
NAME       STATUS   AGE
assets     Active   62s
carts      Active   62s
catalog    Active   7m17s
checkout   Active   62s
orders     Active   62s
other      Active   62s
rabbitmq   Active   62s
ui         Active   62s
```

We can also see all of the Deployments created for the components:

```bash
$ kubectl get deployment -l app.kubernetes.io/created-by=eks-workshop -A
NAMESPACE   NAME             READY   UP-TO-DATE   AVAILABLE   AGE
assets      assets           1/1     1            1           90s
carts       carts            1/1     1            1           90s
carts       carts-dynamodb   1/1     1            1           90s
catalog     catalog          1/1     1            1           7m46s
checkout    checkout         1/1     1            1           90s
checkout    checkout-redis   1/1     1            1           90s
orders      orders           1/1     1            1           90s
orders      orders-mysql     1/1     1            1           90s
ui          ui               1/1     1            1           90s
```

Now that the sample application has been deployed, it is ready to serve as a foundation for the remaining labs in this workshop!

:::tip
If you want to understand more about Kustomize take a look at the [optional module](../kustomize.md) provided in this workshop.
:::