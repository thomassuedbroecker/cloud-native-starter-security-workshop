# Setup Istio

Normally in a production size Kubernetes cluster on IBM Cloud we would install Istio as an Add-On. There are 5 Kubernetes add-ons available: Istio, Knative, Kubernetes Terminal, Diagnostic and Debug Tools, and Static Route. 

Istio installed via the add-on is a managed service and it creates a production grade Istio instance and it requires a cluster with at least 3 worker nodes with 4 CPUs and 16 GB of memory which our lab Kubernetes cluster doesn't have.

Instead, in this lab we will install the Istio demo profile manually using `istioctl` and its standalone operator. `istioctl is available in IBM Cloud Shell, when I wrote these instructions it was at version 1.5.4 which means we will install Istio 1.5.4.

### Step 1: Get the cluster name, cluster ip@ and save it in the `local.env` file.

* Change to the IKS folder
```sh
  cd $ROOT_FOLDER/IKS
```
* Get the environment information
****<<<funktioniert nur im Lab (1 cluster)>>>****
```sh
  sh get-env.sh    
```

* Save the needed environment variables in the `local.env` file
```sh
  source local.env
```

### Step 2: Setup Istio with an operator 

The following commands do install the Istio operator, create a namespace for the Istio backplane, and start to install the Istio backplane.

* Operator
```sh
istioctl operator init
```

* Namespace
```sh
kubectl create ns istio-system
```

* Istio installation
```sh
kubectl apply -f istio.yaml
```

### Step 2: Check the status of Istio installation

```sh
kubectl get pod -n istio-system
```

When install completed the result should look like this:

```sh
NAME                                    READY   STATUS    RESTARTS   AGE
 grafana-5cc7f86765-65fc6                1/1     Running   0          3m28s
 istio-egressgateway-5c8f9897f7-s8tfq    1/1     Running   0          3m32s
 istio-ingressgateway-65dd885d75-vrcg8   1/1     Running   0          3m29s
 istio-tracing-8584b4d7f9-7krd2          1/1     Running   0          3m13s
 istiod-7d6dff85dd-29mjb                 1/1     Running   0          3m29s
 kiali-696bb665-8rrhr                    1/1     Running   0          3m12s
 prometheus-564768879c-2r87j             2/2     Running   0          3m12s
```