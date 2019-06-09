# Inlets Helm Chart
[Inlets](https://github.com/alexellis/inlets) combines a reverse proxy and websocket tunnels to expose your internal and development endpoints to the public Internet via an exit-node made by [Alex Ellis](https://github.com/alexellis)

## Install inlets
```
# Install to /usr/local/bin/
curl -sLS https://get.inlets.dev | sudo sh
```
Check more info at [inlets repository](https://github.com/alexellis/inlets#get-started-install-the-cli)


## Sample values.yaml for deployment without SSL
```
inlets_token: "changeme"

ingress:
  host: inlets.example.com
```

## Sample values.yaml for deployment with SSL
```
inlets_token: "changeme"

ingress:
  tls: true
  annotations:
    ingress.kubernetes.io/ssl-redirect: "true"
    certmanager.k8s.io/issuer: myissuer
  host: inlets.example.com
```

## Render template without tiller
```
git clone https://github.com/teamserverless/inlets-chart
mkdir manifests
helm template --values values.yaml --output-dir ./manifests --name demo ./inlets_helm
kubectl apply -f manifests/inlets/templates/
```

## Testing inlets

Start local service:
```
docker run -p 3000:80 kennethreitz/httpbin
```

Start inlets client:
```
inlets client --remote=wss://inlets.example.com:443  --upstream=http://127.0.0.1:3000 --token="changeme"
```

Check inlets works:
```
curl https://inlets.example.com
```
