# uclab-dev-staging

## Talos Create Cluster
```bash
omnictl cluster template sync -f cluster-template/cluster.yaml
```

```bash
❯ omnictl get clusters
NAMESPACE   TYPE      ID                  VERSION   TALOS VERSION   KUBERNETES VERSION
default     Cluster   uclab-dev-staging   14        1.13.8          1.36.3

❯ omnictl get machines
NAMESPACE   TYPE      ID                                     VERSION   ADDRESS                                   CONNECTED
default     Machine   57c81c42-c74e-9d3e-749a-83ad74c1f6a4   6         fdae:41e4:649b:9303:6b28:964c:3402:24f5   true
default     Machine   a0ba1c42-257c-06b1-34d9-62300ca5861f   6         fdae:41e4:649b:9303:1f5e:987a:5c57:71ed   true
default     Machine   e86e1c42-5982-d568-25f5-435fd42bd8cb   6         fdae:41e4:649b:9303:1c5e:f945:82b9:ae66   true
```

```bash
❯ k get nodes
NAME                                      STATUS     ROLES           AGE   VERSION
uclab-dev-staging-control-planes-4h529s   NotReady   control-plane   94s   v1.36.3
uclab-dev-staging-worker-5rndff           NotReady   <none>          85s   v1.36.3
uclab-dev-staging-worker-hnn9pw           NotReady   <none>          81s   v1.36.3
```

## CNI

### Gateway API CRDs first, both standard and experimental channels, applied via plain kubectl apply:
```bash
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.6.1/standard-install.yaml
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.6.1/experimental-install.yaml
```



### Cilium via cilium-cli install
```bash
cilium install \
--version 1.20.1 \
--set cluster.name=uclab-dev-staging \
--values cilium-values.yaml
```


```bash
❯ cilium status --wait
    /¯¯\
 /¯¯\__/¯¯\    Cilium:             OK
 \__/¯¯\__/    Operator:           OK
 /¯¯\__/¯¯\    Envoy DaemonSet:    OK
 \__/¯¯\__/    Hubble Relay:       OK
    \__/       ClusterMesh:        disabled

DaemonSet              cilium                   Desired: 3, Ready: 3/3, Available: 3/3
DaemonSet              cilium-envoy             Desired: 3, Ready: 3/3, Available: 3/3
Deployment             cilium-operator          Desired: 1, Ready: 1/1, Available: 1/1
Deployment             hubble-relay             Desired: 1, Ready: 1/1, Available: 1/1
Deployment             hubble-ui                Desired: 1, Ready: 1/1, Available: 1/1
Containers:            cilium                   Running: 3
                       cilium-envoy             Running: 3
                       cilium-operator          Running: 1
                       clustermesh-apiserver
                       hubble-relay             Running: 1
                       hubble-ui                Running: 1
Cluster Pods:          6/6 managed by Cilium
Helm chart version:    1.20.1
Image versions         cilium             quay.io/cilium/cilium:v1.20.1@sha256:ae9ea21f7427fe24bc6ea7247eb552157a1b0a431744045d3f641545ca71d11b: 3
                       cilium-envoy       quay.io/cilium/cilium-envoy:v1.37.5-1786810558-766ccfb37260a43e9d228837aa84ce3faf9f64e7@sha256:75b8094c7127736a2ffd2dce3945e0931cb6df21b0372ff661940eca26730b91: 3
                       cilium-operator    quay.io/cilium/operator-generic:v1.20.1@sha256:6c3885fc7b629099fdbe2a5c87869c86feb825fa18fae299eac0f61918d16ecf: 1
                       hubble-relay       quay.io/cilium/hubble-relay:v1.20.1@sha256:59be0ae7d475ab9011a5e954618c0f27b5778b17140381425b308b55ba4917f4: 1
                       hubble-ui          quay.io/cilium/hubble-ui-backend:v0.13.5@sha256:fac0c300ae119274edca11fd89b1ad23c788792d8bc4ea2ba631c709e8d3c688: 1
                       hubble-ui          quay.io/cilium/hubble-ui:v0.13.5@sha256:f7d514fc54d784ed6df9d58cf0e97648b143f92b766dd1780ed3fc845bd4c516: 1

❯ k get nodes -o wide
NAME                                      STATUS   ROLES           AGE     VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE          KERNEL-VERSION          CONTAINER-RUNTIME
uclab-dev-staging-control-planes-4h529s   Ready    control-plane   4m13s   v1.36.3   10.10.10.197   <none>        Talos (v1.13.8)   6.18.42-talos (amd64)   containerd://2.2.6
uclab-dev-staging-worker-5rndff           Ready    <none>          4m4s    v1.36.3   10.10.10.195   <none>        Talos (v1.13.8)   6.18.42-talos (amd64)   containerd://2.2.6
uclab-dev-staging-worker-hnn9pw           Ready    <none>          4m      v1.36.3   10.10.10.193   <none>        Talos (v1.13.8)   6.18.42-talos (amd64)   containerd://2.2.6
```

