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
