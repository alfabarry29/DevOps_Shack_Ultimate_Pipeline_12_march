# DevOps Demo App

This is a small sample app used to demonstrate a CI/CD pipeline in Kubernetes.

## Run locally

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

Then open http://localhost:8080

## Build Docker image

```bash
docker build -t devops-demo .
docker run -p 8080:8080 devops-demo
```

## Kubernetes deployment

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```
