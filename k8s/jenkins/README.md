# Jenkins

This folder contains Kubernetes manifests to deploy Jenkins as a CI/CD server.

## Documentation

- Official Jenkins Kubernetes guide: https://www.jenkins.io/doc/book/installing/kubernetes/
- Official Docker image: https://hub.docker.com/r/jenkins/jenkins

## Deployment

```sh
kubectl apply -f k8s/jenkins/namespace.yaml \
              -f k8s/jenkins/jenkins-pvc.yaml \
              -f k8s/jenkins/jenkins-deployment.yaml \
              -f k8s/jenkins/jenkins-service.yaml
kubectl -n jenkins rollout status deploy/jenkins-deployment
kubectl -n jenkins port-forward svc/jenkins-service 8081:8090
```

Then access Jenkins at http://localhost:8081/.

## Credentials

Retrieve the initial admin password:

```sh
kubectl -n jenkins exec deploy/jenkins-deployment -- cat /var/jenkins_home/secrets/initialAdminPassword
```
