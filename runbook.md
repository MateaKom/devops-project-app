# Runbook za incidente - Secure Event Ticketing Platform

## 1. Pad baze podataka (PostgreSQL)

### Simptomi
- API vraca gresku Unable to read orders
- Worker ne procesira narudžbe

### Dijagnoza
kubectl get pods
kubectl logs deployment/postgres

### Rješenje
kubectl rollout restart deployment/postgres
kubectl exec postgres-pod -- psql -U ticketing_user -d ticketing -c "SELECT 1"

## 2. Los image tag

### Simptomi
- Pod u statusu ErrImagePull ili ImagePullBackOff

### Dijagnoza
kubectl get pods
kubectl describe pod pod-name

### Rješenje
kubectl set image deployment/name name=image:correct-tag
kubectl rollout status deployment/name

## 3. Neispravan Secret

### Simptomi
- Servisi se ne mogu spojiti na bazu
- Greška authentication failed

### Dijagnoza
kubectl get secrets
kubectl describe secret postgres-secret

### Rješenje
kubectl delete secret postgres-secret
kubectl apply -f k8s/postgres/secret.yaml
kubectl rollout restart deployment/api
kubectl rollout restart deployment/worker

## 4. Rolling Update i Rollback

### Rolling Update
docker build -t devops-project-app-api:v2 ./api
kubectl set image deployment/api api=devops-project-app-api:v2
kubectl rollout status deployment/api

### Rollback
kubectl rollout undo deployment/api
kubectl rollout status deployment/api

## 5. Provjera logova
kubectl logs deployment/api
kubectl logs deployment/worker
kubectl logs deployment/frontend
kubectl logs deployment/postgres
