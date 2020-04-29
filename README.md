# TANZU APPLICATION SERVIVCE FOR KUBERNETES
---

## Prerequisits 
* `kubectl`
* `bosh`
* `kapp`
* `kbld`
* `ytt`
* `cf`

Make sure the default storage class is present -

```kubectl get storageclass```

```
cd tas
mv custom-overlays/replace-loadbalancer-with-clusterip.yaml config-optional
```
```
cd tas
./bin/generate-values.sh -d "sys.navneetv.com" > ../configuration-values/deployment-values.yml
```

```
./bin/install-tas.sh ../configuration-values
```

Get the LB and update DNS

```
kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}
or
kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}
```

