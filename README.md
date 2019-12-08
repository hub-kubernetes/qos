# Quality of Service (QOS)

When Kubernetes creates a Pod it assigns one of these QoS classes to the Pod:

* Guaranteed
* Burstable
* BestEffort


Lets start off by creating a namespace for this demo - 

` kubectl create namespace qos ` 

--- 

## Pod with Guaranteed QOS 

For a Pod to be given a QoS class of Guaranteed:

* Every Container in the Pod must have a memory limit and a memory request, and they must be the same.
* Every Container in the Pod must have a CPU limit and a CPU request, and they must be the same.

An example could be - 

```
apiVersion: v1
kind: Pod
metadata:
  name: qos-demo
  namespace: qos
spec:
  containers:
  - name: qos-demo-ctr
    image: nginx
    resources:
      limits:
        memory: "200Mi"
        cpu: "700m"
      requests:
        memory: "200Mi"
        cpu: "700m"

```

To view the QOS - 

```
get pod qos-demo --namespace=qos -o yaml | grep -i qosclass
  qosClass: Guaranteed
```


--- 

## Pod with Burstable QOS Class

A Pod is given a QoS class of Burstable if:

* The Pod does not meet the criteria for QoS class Guaranteed.
* At least one Container in the Pod has a memory or CPU request.

```
apiVersion: v1
kind: Pod
metadata:
  name: qos-demo-2
  namespace: qos
spec:
  containers:
  - name: qos-demo-2-ctr
    image: nginx
    resources:
      limits:
        memory: "200Mi"
      requests:
        memory: "100Mi"

```

To view the QoS

```
kubectl get pods qos-demo-2 -n qos -o yaml | grep -i qosclass
  qosClass: Burstable
```

---

## Pod with BestEffort QOS Class

For a Pod to be given a QoS class of BestEffort, the Containers in the Pod must not have any memory or CPU limits or requests.

```
apiVersion: v1
kind: Pod
metadata:
  name: qos-demo-3
  namespace: qos
spec:
  containers:
  - name: qos-demo-3-ctr
    image: nginx

```

To view the qos class 

```
kubectl get pods qos-demo-3 -n qos -o yaml | grep -i qosclass
  qosClass: BestEffort
```
