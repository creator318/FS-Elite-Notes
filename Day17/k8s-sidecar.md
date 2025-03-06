#SE/Kubernetes #Design/Sidecar

# Kubernetes Deployment Guide for Flask App and Logger

------

## Scenario

A company is develop-ing a **Medical Billing System** that exposes REST APIs to manage billing records. The system consists of two components:

1. **Flask App (app.py)** – Provides REST API endpoints for billing management.  
2. **Logger Service (logger.py)** – Monitors application logs in real time.

To ensure **scalability, reliability, and automation**, the application needs to be deployed on **Kubernetes**. The deployment should allow for:

- **Containerization**: Package both the Flask app and logger into Docker containers.
- **Private Docker Registry**: Store and manage container images in a local registry.
- **Deployment in Kubernetes**: Deploy and manage the containers in a Kubernetes cluster.
- **Networking**: Expose the Flask app as a service while keeping the logger internal.
- **Scalability**: Allow the system to scale by increasing replicas.
- **Logging**: Enable centralized logging using a shared volume for logs.
- **Service Exposure**: Allow external access to the Flask app through a Kubernetes NodePort service.


## Solution Approach

### Step 1: Setup a Local Private Docker Registry

1. **Start a local registry container** to store images: `docker run -d -p 5000:5000 --name registry registry:2`
2. **Verify the registry is running:**: `docker ps | grep registry`
    
### Step 2: Containerizing the Applications

1. **Create a Dockerfile.app for App (app.py)**
	```
	FROM python:3.13-slim
	
	WORKDIR /app
	
	COPY app.py /app
	
	RUN pip install --no-cache-dir flask
	
	RUN mkdir -p /logs && chmod 777 /logs
	
	EXPOSE 80
	
	CMD ["python", "app.py"]
	```

2. **Create a Dockerfile.logger for the Logger (logger.py)**
	```
	FROM python:3.13-slim
	
	WORKDIR /logger
	
	COPY logger.py /logger/
	
	RUN mkdir -p /logs && chmod 777 /logs
	
	CMD ["python", "logger.py"]
	```

### Step 3: Build and Push Docker Images to Local Registry

1. **Build Docker Images:** 
	```
	docker build -t sidecar-app -f Dockerfile.app .
	docker build -t sidecar-logger -f Dockerfile.logger .
	```

2. **Tag Images for Local Registry:** 
	```
	docker tag sidecar-app localhost:5000/sidecar-app
	docker tag sidecar-logger localhost:5000/sidecar-logger
	```

3. **Push Images to Local Registry:** 
	```
	docker push localhost:5000/sidecar-app
	docker push localhost:5000/sidecar-logger
	```

4. **Verify Images in Registry:**: `curl http://localhost:5000/v2/_catalog`

### Step 4: Kubernetes Deployment YAML Files

1. **Deploy Flask Application and Logger - deployment.yaml**

	```yml
	apiVersion: apps/v1
	kind: Deployment
	metadata:
		name: sidecar-app
	spec:
		replicas: 1
		selector:
			matchLabels:
				app: sidecar-app
		template:
			metadata:
				labels:
					app: sidecar-app
			spec:
				containers:
				- name: sidecar-app
					image: sidecar-app:latest
					imagePullPolicy: Never
					ports:
					- containerPort: 80
					volumeMounts:
					- name: log-volume
						mountPath: /logs
				- name: sidecar-logger
					image: sidecar-logger:latest
					imagePullPolicy: Never
					volumeMounts:
					- name: log-volume
						mountPath: /logs
				volumes:
				- name: log-volume
					emptyDir: {}
	```

2. **Expose Flask App as a Service - service.yaml**

	```yml
	apiVersion: v1
	kind: Service
	metadata:
		name: sidecar-app
	spec:
		type: NodePort
		selector:
			app: sidecar-app
		ports:
			- protocol: TCP
				port: 80
				targetPort: 80
				nodePort: 30001
	```

### Step 5: Deploying on Kubernetes

1. **Apply the Deployments & Services** 
	```
	kubectl apply -f deployment.yaml
	kubectl apply -f service.yaml
	```

2. **Verify Pods and Services** 
	```
	kubectl get pods
	kubectl get services
	```

3. **Access the Flask App** 
	```
	kubectl get nodes
	curl http://<node-IP>:30001
	```

### Step 6: Monitoring and Cleanup **(Optional)**

- **Monitor Logs from Logger**: `kubectl logs -f deployment/logger`
- **Scale Up the Flask App**: `kubectl scale deployment sidecar-app --replicas=3`
- **Delete the Deployment** 
	```
	kubectl delete -f deployment.yaml
	kubectl delete -f service.yaml
	```
- **Delete All Resources**: `kubectl delete all --all`