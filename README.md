# Inlets Helm Chart
[Inlets](https://github.com/alexellis/inlets) combines a reverse proxy and websocket tunnels to expose your internal and development endpoints to the public Internet via an exit-node made by [Alex Ellis](https://github.com/alexellis)

## Install inlets
```
# Install to /usr/local/bin/
curl -sLS https://get.inlets.dev | sudo sh
```
Check more info at [inlets repository](https://github.com/alexellis/inlets#get-started-install-the-cli)


## Sample values.yaml
```
ws_ingress:
  host: wsinlets.paurosello.apps.beta.k8spin.cloud

ingress:
  annotations:
    certmanager.k8s.io/issuer: paurosello-gmail-com
  host: inlets.paurosello.apps.beta.k8spin.cloud
```

## Render template without tiller
```
git clone https://github.com/paurosello/inlets_helm.git
mkdir manifests
helm template --values values.yaml --output-dir ./manifests --name k8spin ./inlets_helm
kubectl apply -f manifests/inlets/templates/
```

## Testing inlets

Start local service:
```
docker run -p 3000:80 kennethreitz/httpbin
```

Start inlets client:
```
inlets client --remote=ws://wsinlets.paurosello.apps.beta.k8spin.cloud:80  --upstream=http://127.0.0.1:3000
```