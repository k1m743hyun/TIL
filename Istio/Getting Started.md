# Getting Started


## Download istio


### Using Brew  

```
$ brew install istioctl
```


### Using Curl  


#### Download the istio installation file for your OS  

- latest release  

```
$ curl -L https://istio.io/downloadIstio | sh -
```

- istio 1.15.0 for the x86_64 architecture  

```
$ curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.15.0 TARGET_ARCH=x86_64 sh -
```


#### Move to the istio package directory

```
$ cd istio-1.15.0
```


#### Add the `istioctl` client to your path

```
$ export PATH=$PWD/bin:$PATH 
```


## Install istio

- Using demo configuration file

```
$ istioctl install --set profile=demo -y
```

- Add a namespace label to instruct istio to automatically inject Envoy sidecar proxies

```
$ kubectl label namespace default istio-injection=enabled
```


## Deploy the sample application


### Deploy the `Bookinfo` sample application

- Using Brew when you installed

```
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.5/samples/bookinfo/platform/kube/bookinfo.yaml
```

- Using Curl when you installed
```
$ kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```


### Check the sample application is working

- service

```
$ kubectl get service -n default
```

- pod

```
$ kubectl get pod -n defalut
```


### Verify the sample application is working correctly up

```
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```


# 출처
- [Getting Started](https://istio.io/latest/docs/setup/getting-started/)
- [Istio (이스티오) 튜토리얼](https://medium.com/@chanhyeonglee/istio-%EC%9D%B4%EC%8A%A4%ED%8B%B0%EC%98%A4-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-84d7d4b8eab)