# Jenkins Deployment on Kubernetes using Helm

## Prerequisites

* Kubernetes Cluster
* kubectl configured
* Helm installed
* Docker Hub account and Personal Access Token (PAT)

---

## 1. Create Namespace

```bash
kubectl create namespace namespace
```

Verify:

```bash
kubectl get ns
```

---

## 2. Create Docker Registry Secret

Create the image pull secret for Docker Hub:

```bash
kubectl create secret docker-registry regcred \
  --docker-username=<docker-username> \
  --docker-password=<docker-pat> \
  --docker-email=<email> \
  -n namespace
```

Verify:

```bash
kubectl get secrets -n namespace
```

Describe secret:

```bash
kubectl describe secret regcred -n namespace
```

---

## 3. Deploy Jenkins using Helm

Move to the Helm chart directory and deploy:

```bash
helm upgrade --install my-release . -n namespace
```

Check Helm releases:

```bash
helm list -n namespace
```

Check release status:

```bash
helm status my-release -n namespace
```

---

## 4. Verify Deployment

Get all resources:

```bash
kubectl get all -n namespace
```

Get pods:

```bash
kubectl get pods -n namespace
```

Get services:

```bash
kubectl get svc -n namespace
```

Get deployments:

```bash
kubectl get deployments -n namespace
```

Get replica sets:

```bash
kubectl get rs -n namespace
```

Describe pod:

```bash
kubectl describe pod <pod-name> -n namespace
```

View pod logs:

```bash
kubectl logs <pod-name> -n namespace
```

Follow logs:

```bash
kubectl logs -f <pod-name> -n namespace
```

---

## 5. Access Jenkins

Port forward Jenkins service:

```bash
kubectl port-forward svc/<service-name> 8080:8080 -n namespace
```

Access Jenkins:

```text
http://localhost:8080
```

Get Jenkins initial admin password:

```bash
kubectl exec -it <pod-name> -n namespace -- cat /var/jenkins_home/secrets/initialAdminPassword
```

---

## 6. Upgrade Release

```bash
helm upgrade my-release . -n namespace
```

---

## 7. Uninstall Release

```bash
helm uninstall my-release -n namespace
```

Verify:

```bash
kubectl get all -n namespace
```

---

## Useful Commands

```bash
kubectl get pods -A
kubectl get svc -A
kubectl get deployments -A
kubectl get pvc -A
kubectl get pv
kubectl get events -n namespace
kubectl top pods -n namespace
kubectl top nodes
```

### Helm Commands

```bash
helm list -A
helm status my-release -n namespace
helm history my-release -n namespace
helm get values my-release -n namespace
helm get manifest my-release -n namespace
```
