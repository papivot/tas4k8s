# TANZU APPLICATION SERVIVCE FOR KUBERNETES
---

## Prerequisits 
* `kubectl`
* `bosh`
* `kapp`
* `kbld`
* `ytt`
* `cf`
---

### Make sure the default storage class is present -

```kubectl get storageclass```
### To use service type loadbalancer -
```
cd tas
mv custom-overlays/replace-loadbalancer-with-clusterip.yaml config-optional
```
### Generate the configuration file with self signed certs. 
```
cd tas
./bin/generate-values.sh -d "sys.navneetv.com" > ../configuration-values/deployment-values.yml
```
### Kickstart the install and wait for 5-7 mins. 
```
./bin/install-tas.sh ../configuration-values
```
### Get the LB and update DNS
```
kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].hostname}
or
kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}
```
Use the value to update DNS with *.sys.navneetv.com

### Login 
```
cd tas
cf api api.sys.navneetv.com --skip-ssl-validation
CF_ADMIN_PASSWORD="$(bosh interpolate ../configuration-values/deployment-values.yml --path /cf_admin_password)"
cf auth admin "$CF_ADMIN_PASSWORD"
cf enable-feature-flag diego_docker
```

### Create Org and Spaces
```
cf create-org test-org
cf create-space -o test-org test-space
cf target -o test-org -s test-space
```

### Download and cf push app
```
cd ..
git clone https://github.com/cloudfoundry-samples/test-app.git
cd test-app
cf push test-app --hostname test-app
cf logs test-app
curl test-app.sys.navneetv.com
```

### Delete TAS
```
kapp delete -a cf
```
---
---
Edit this file 
./config/cf-for-k8s/config/_ytt_lib/github.com/cloudfoundry/cf-k8s-networking/config/istio-generated/xxx-generated-istio.yaml

Once setup get the Loadbalancer IP and update the DNS - 
```
192.168.10.163 sys.navlab.io api.sys.navlab.io login.sys.navlab.io uaa.sys.navlab.io test-app.sys.navlab.io log-cache.sys.navlab.io
```





