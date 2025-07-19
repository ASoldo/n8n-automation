# N8N Automation & Kubernetes Notes

## Addon Ingress

```sh
minikube addons enable ingress
```sh

## Docker Registry on Minikube

Deploy a simple Docker registry (registry:2) with persistent storage and Ingress:

```sh
kubectl apply -f k8s/harbor/namespace.yaml \
              -f k8s/harbor/registry-pvc.yaml \
              -f k8s/harbor/registry-deployment.yaml \
              -f k8s/harbor/registry-service.yaml \
              -f k8s/harbor/registry-ingress.yaml
```

Optionally add the following to `/etc/hosts` so `harbor.local` resolves to Minikube:

```sh
echo "$(minikube ip) harbor.local" | sudo tee -a /etc/hosts
```

You can then login and push images using Podman:

```sh
podman login harbor.local
podman tag <your-image>:<tag> harbor.local/<your-image>:<tag>
podman push harbor.local/<your-image>:<tag>
```

To list the repositories in the registry catalog:

```sh
curl -X GET http://harbor.local:5000/v2/_catalog
```
```

## Argo CD

Install Argo CD into the `argocd` namespace:

```bash
kubectl apply -f k8s/argocd/namespace.yaml
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Addon Metrics System

```sh
minikube addons enable metrics-server

```

```sh
 rootster@archiebald  ~  curl http://ollama.local/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "mistral:latest",
    "prompt": "What is DevOps?",
    "stream": false
}'
```

## n8n Token

```sh
eyJhbGciOiJSUzI1NiIsImtpZCI6ImNQRkJ2d2N6VDVFakFiUnp2ZDFWajRTazRtZGpvNzNpVWxyZXRtX2gxNkUifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzc3ODQ3ODQxLCJpYXQiOjE3NDYzMTE4NDEsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwianRpIjoiYTg1ZDEyNzAtOTFlYS00Y2U1LThjMzEtOWMwZGI0YjYzNzY3Iiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJhdXRvbWF0aW9uIiwibm9kZSI6eyJuYW1lIjoibWluaWt1YmUiLCJ1aWQiOiJlZTE1Zjk5My0zNzVmLTRhMjAtYWIzNC01YmY0ZWQ2NjM3YjAifSwicG9kIjp7Im5hbWUiOiJuOG4tNzVmNjg0YmM0LWdmcWhiIiwidWlkIjoiMDFhZDlhN2QtYWI0ZS00YjgzLTkwOWQtZGQ4OTAyZGIxZGM0In0sInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJuOG4tc2EiLCJ1aWQiOiIwZjJiNmZmOC03NjA1LTRmOGQtODJmZi0wZWM1OGVjNTZiNDgifSwid2FybmFmdGVyIjoxNzQ2MzE1NDQ4fSwibmJmIjoxNzQ2MzExODQxLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6YXV0b21hdGlvbjpuOG4tc2EifQ.KY4R8V5rxpp6gUBmbT4IOd9yytRt6S1nsWgPV14uMVJw-BoWVtj_boFz6Vw0EtbGDfIXl5CCO5EmGZI4cQjvVfgwR5EdHyzhSMPXzPWf9pdr4xB8g68VWkM9BLteKnJ6FIAZaykpehjJMMIa1is9gY9-enVieMkJCNqCvd3xIzvHW92o01c0qhTGfhT0xeD92i2FrA2ZuxO_qZzn0yHuCoZH5sVNwnEPPsK5hPQxYV6CeEk99DASubmdODSeWaVjggGa2PpK0FXPAUAemCfD5aVD04lQparfnVfOlaKk_9MS8Gg1jpBDz4m0QUwsgAUp4CJCIvQbOgxuHKPskyQ2Sg
```

## Curl

Url:

```sh
https://kubernetes.default.svc/api/v1/pods
```

Header Parameters

```sh
Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6ImNQRkJ2d2N6VDVFakFiUnp2ZDFWajRTazRtZGpvNzNpVWxyZXRtX2gxNkUifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzc3ODQ3ODQxLCJpYXQiOjE3NDYzMTE4NDEsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwianRpIjoiYTg1ZDEyNzAtOTFlYS00Y2U1LThjMzEtOWMwZGI0YjYzNzY3Iiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJhdXRvbWF0aW9uIiwibm9kZSI6eyJuYW1lIjoibWluaWt1YmUiLCJ1aWQiOiJlZTE1Zjk5My0zNzVmLTRhMjAtYWIzNC01YmY0ZWQ2NjM3YjAifSwicG9kIjp7Im5hbWUiOiJuOG4tNzVmNjg0YmM0LWdmcWhiIiwidWlkIjoiMDFhZDlhN2QtYWI0ZS00YjgzLTkwOWQtZGQ4OTAyZGIxZGM0In0sInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJuOG4tc2EiLCJ1aWQiOiIwZjJiNmZmOC03NjA1LTRmOGQtODJmZi0wZWM1OGVjNTZiNDgifSwid2FybmFmdGVyIjoxNzQ2MzE1NDQ4fSwibmJmIjoxNzQ2MzExODQxLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6YXV0b21hdGlvbjpuOG4tc2EifQ.KY4R8V5rxpp6gUBmbT4IOd9yytRt6S1nsWgPV14uMVJw-BoWVtj_boFz6Vw0EtbGDfIXl5CCO5EmGZI4cQjvVfgwR5EdHyzhSMPXzPWf9pdr4xB8g68VWkM9BLteKnJ6FIAZaykpehjJMMIa1is9gY9-enVieMkJCNqCvd3xIzvHW92o01c0qhTGfhT0xeD92i2FrA2ZuxO_qZzn0yHuCoZH5sVNwnEPPsK5hPQxYV6CeEk99DASubmdODSeWaVjggGa2PpK0FXPAUAemCfD5aVD04lQparfnVfOlaKk_9MS8Gg1jpBDz4m0QUwsgAUp4CJCIvQbOgxuHKPskyQ2Sg
```


```sh
{{ $node["When chat message received"].json.chatInput + " " + JSON.stringify($json.items) }}

```

```sh
{{ 
  $node["When chat message received"].json.chatInput + ": " + 
  $json.items.map(p => `Pod "${p.metadata.name}" in namespace "${p.metadata.namespace}" is ${p.status.phase} with IP ${p.status.podIP}`).join("\n")
}}

```

```sh
{{ 
  $node["When chat message received"].json.chatInput + ":\n\n" + 
  $json.items.map(p => 
    `${p.metadata.name} (${p.metadata.namespace})
  - Created: ${p.metadata.creationTimestamp}
  - Status: ${p.status.phase}
  - IP: ${p.status.podIP}
  - Container: ${p.status.containerStatuses?.[0]?.name || "N/A"}
  - Image: ${p.status.containerStatuses?.[0]?.image || "N/A"}
  - Restarts: ${p.status.containerStatuses?.[0]?.restartCount ?? 0}
  - Started: ${p.status.startTime}`
  ).join("\n\n")
}}

```

```sh
{{$json.summary + ":\n\n" + $json.pods.map(p => 
`${p.name} (${p.namespace})
  - Created: ${p.created}
  - Status: ${p.status}
  - IP: ${p.ip}
  - Container: ${p.container}
  - Image: ${p.image}
  - Restarts: ${p.restarts}
  - Started: ${p.started}`).join("\n\n")}}

```

```js
Pod Names:
{{ $('get-metrics-system').item.json.items.map(item => item.metadata.name).join('\n') }}

Pod Namespaces:
{{ $('get-metrics-system').item.json.items.map(item => item.metadata.namespace).join('\n') }}

Pod containers:
{{ $('get-metrics-system').item.json.items.map(item => item.containers[0].name).join('\n') }}

Containers Usage CPU:
{{ $('get-metrics-system').item.json.items.map(item => item.containers[0].usage.cpu).join('\n') }}

Containers Usage Memory:
{{ $('get-metrics-system').item.json.items.map(item => item.containers[0].usage.memory).join('\n') }}

```

## Mount local files to Minikube and use them in n8n

```sh
 minikube mount /home/rootster/Documents/automations/:/automations/
```

or run this in the background:

```sh
 minikube mount /home/rootster/Documents/automations/:/automations/ &
```

## WireGuard Setup

### Generate Server Keys

```sh
wg genkey | tee server.key | wg pubkey > server.pub
```

### Generate Client Keys

```sh
wg genkey | tee client.key | wg pubkey > client.pub
```

Move `wg0` to `/etc/wireguard/`

```sh
[Interface]
PrivateKey = private_key
Address = 10.8.0.2/24
# DNS = 1.1.1.1
Table = off

[Peer]
PublicKey = public_key
Endpoint = 192.168.49.2:30518
AllowedIPs = 10.244.0.0/16
PersistentKeepalive = 25
```


## TwinGate

| Store Tokens on secure location like BitWarden, etc...

```sh
sudo twingate setup

Twingate has been started; user authentication is required for access to Resources
To start desktop notifications, run `twingate desktop-start`.
Alternatively, you can run `/usr/bin/twingate-notifier console` in order to receive Twingate authentication requests in the console.
 rootster@archiebald  ~/Documents/automations/k8s   main ± 
```
