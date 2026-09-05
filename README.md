# Secure Event Ticketing Platform

## Opis projekta
Višeslojna aplikacija za prodaju ulaznica koja demonstrira DevOps/DevSecOps prakse.

## Servisi
- **Frontend** - Web sučelje (port 3000)
- **API** - REST API servis (port 8080)
- **Worker** - Background worker za obradu narudžbi
- **PostgreSQL** - Baza podataka
- **Redis** - Queue i cache

## 1. dio - Lokalni razvoj (Docker Compose)

### Pokretanje
`ash
docker-compose up --build
`

### Zaustavljanje
`ash
docker-compose down
`

### Provjera zdravlja
- Frontend: http://localhost:3000
- API health: http://localhost:8080/healthz
- Narudžbe: http://localhost:8080/tickets/orders

## 2. dio - Produkcijski deployment (Kubernetes)

### Preduvjeti
- Docker Desktop s omogućenim Kubernetesom
- kubectl

### Build slika
`ash
docker build -t devops-project-app-api:latest ./api
docker build -t devops-project-app-frontend:latest ./frontend
docker build -t devops-project-app-worker:latest ./worker
`

### Deploy
`ash
kubectl apply -f k8s/postgres/secret.yaml
kubectl apply -f k8s/app-configmap.yaml
kubectl apply -f k8s/rbac.yaml
kubectl apply -f k8s/postgres/deployment.yaml
kubectl apply -f k8s/redis/deployment.yaml
kubectl apply -f k8s/api/deployment.yaml
kubectl apply -f k8s/worker/deployment.yaml
kubectl apply -f k8s/frontend/deployment.yaml
`

### Provjera statusa
`ash
kubectl get pods
`

### Pristup aplikaciji
`ash
kubectl port-forward service/frontend 3000:3000
kubectl port-forward service/api 8080:8080
`

## Sigurnosno skeniranje
Skeniranje slika vrši se pomoću Trivy alata:
`ash
trivy image devops-project-app-api:latest
trivy image devops-project-app-frontend:latest
trivy image devops-project-app-worker:latest
`

Rezultati su spremljeni u:
- trivy-report-api.json
- trivy-report-frontend.json
- trivy-report-worker.json
