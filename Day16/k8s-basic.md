#SE/Kubernetes

# Deploying a Python Server Application using Kubernetes

------

## Objective :

The goal of this task is to **deploy a Python server application** using **Kubernetes** on **Docker Desktop**. You will containerize the Python application using **Docker**, create a **Kubernetes Deployment**, and expose it via a **NodePort** **service** so that it is accessible from other machines in the network.

### Requirements:

You need to perform the following tasks:
    
1. **Containerize a simple Flask Application using Docker**
	- Write a **Dockerfile** to package the application.
    - Build and test the **Docker image** locally.

2. **Deploy the Application on Kubernetes**
	- Create a **Kubernetes Deployment** (deployment.yaml) to manage the Python server pods.
    - Ensure the Deployment runs exactly **3 replicas**.

3. **Expose the Service using NodePort**
    - Create a **NodePort service** (service.yaml) to make the application accessible.
    - Ensure the application is accessible via `<node-IP>:<NodePort>`.
    
4. **Test the Deployment**
    - Retrieve the **assigned NodePort** and test access using curl or a web browser.
    - Verify the application returns the expected response.

### **Expected Deliverables:**

1. **Python server application (app.py)**
2. **Dockerfile** **to containerize the app**
3. **Kubernetes Deployment YAML (deployment.yaml)**
4. **Kubernetes Service YAML (service.yaml)**
5. **Instructions on how to test the deployed application**

### Success Criteria:

- The Python server should be **running in Kubernetes** on Docker Desktop.
- The application should be **accessible from a browser or via curl** using `<node-IP>:<NodePort>`.
- Running kubectl get pods should show Exactly **3** **running replicas**.

### Additional Notes:

 - Use `kubectl describe service` to find the **NodePort**
 - The service should be accessible using: `curl http://<node-IP>:<NodePort>`
 - If using **Windows**, replace localhost with host.docker.internal if needed.

## Solution Approach:

### Step 1:  Containerize the Flask Application using Docker

1. **Create a Dockerfile** for the Flask App
	```
	FROM python-3.13-slim
	
	WORKDIR /app
	
	COPY app.py /app
	
	RUN pip install --no-cache-dir flask
	
	EXPOSE 80
	
	CMD ["python", "app.py"]
	```
	
2. **Build the Docker Image**: `docker build -t basic-app`

3. **Verify the Docker Image**: `docker images`

4. **Test the image locally**
	- Run the container and test:
		```
		docker run -d -p 8080:80 basic-app --name basic-app
		curl http://localhost:8080/hello
		docker stop basic-app
		```

### Step 2: Push the image to Local Private Docker Registry

1. **Start a local registry container** to store images: `docker run -d -p 5000:5000 --name registry registry:2`
2. **Verify the registry is running:**: `docker ps | grep registry`
3. **Tag image fot Local Registy**: `docker tag basic-app localhost:5000/basic-app`
4. **Push the image to the local registry**: `docker push localhost:5000/basic-app`
5. **Verify Images in Registry:**: `curl http://localhost:5000/v2/_catalog`

### Step 3: Kubernetes Deployment YAML Files

1. **Deploy the Flask Application - deployment.yaml**

	```yml
	apiVersion: apps/v1
 	kind: Deployment
 	metadata:
 		name: basic-app
 	spec:
 		replicas: 3
 		selector:
 			matchLabels:
 				app: basic-app
 		template:
 			metadata:
 				labels:
 					app: basic-app
 			spec:
 				containers:
 				- name: basic-app
 					image: basic-app:latest
 					imagePullPolicy: Never
 					ports:
 					- containerPort: 80
	```

2. **Expose Flask App as a Service - service.yaml**

	```yml
	apiVersion: v1
	kind: Service
	metadata:
		name: basic-app-service
	spec:
		type: NodePort
		selector:
			app: basic-app
		ports:
		- protocol: TCP
			port: 80
			targetPort: 80
			nodePort: 30000
	```

### Step 4: Deploying on Kubernetes

1. **Apply the Deployments & Services** 
	```
	kubectl apply -f deployment.yaml
	kubectl apply -f service.yaml
	```

2. **Verify Pods, Replicas and Services** 
	```
	kubectl get pods
	kubectl get deployment python-server
	kubectl get services
	```

3. **Access the Flask App** 
	```
	kubectl get nodes
	curl http://<node-IP>:30000
	```

### Step 5: Cleanup **(Optional)**

- **Scale Up the Flask App**: `kubectl scale deployment basic-app --replicas=3`
- **Delete the Deployment**:  
	```
	kubectl delete -f deployment.yaml
	kubectl delete -f service.yaml
	```
- **Delete All Resources**: `kubectl delete all --all`