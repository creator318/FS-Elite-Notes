#SE/Kubernetes 

# Setting Up Kubernetes Dashboard on Docker-Desktop Cluster

------

The **Kubernetes Dashboard** is a web-based UI for managing Kubernetes clusters. Follow these steps to deploy and access it on a Docker Desktop based cluster.

## Step 1: Deploy the Kubernetes Dashboard

Apply the official **Kubernetes Dashboard** YAML file: 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
This deploys the Dashboard, its services, and necessary components.

## Step 2: Verify Deployment

Check if the kubernetes-dashboard pod is running: 
```
kubectl get pods -n kubernetes-dashboard
```

## Step 3: Create Admin User & Role Binding

By default, the Dashboard has limited access. To enable full control, create a service account and bind it to the cluster-admin role.

admin-user.yaml
```yml
apiVersion: v1
kind: ServiceAccount
metadata:
	name: dashboard-user
	namespace: kubernetes-dashboard
```

cluster-rolebinding.yaml
```yml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
	name: dashboard-user
roleRef:
	apiGroup: rbac.authorization.k8s.io
	kind: ClusterRole
	name: cluster-admin
subjects:
- kind: ServiceAccount
	name: dashboard-user
	namespace: kubernetes-dashboard
```

  **Apply the Roles :** 
```
kubectl apply -f dashboard-admin.yaml
kubectl apply -f cluster-rolebinding.yaml
```

## Step 4: Get the Authentication Token

To log in to the Dashboard, fetch the token for the dashboard-user:

```
kubectl -n kubernetes-dashboard create token dashboard-user
```

Copy this token for later use.

## Step 5: Access the Dashboard

Expose via NodePort **(Remote Access)**

If you need to access the Dashboard externally:

1. Edit the Dashboard service:
	
	```
	kubectl -n kubernetes-dashboard edit service kubernetes-dashboard
	```

2. Change `type: ClusterIP` to `type: NodePort` and save.

3. Get the assigned NodePort:

	```
	kubectl -n kubernetes-dashboard get svc kubernetes-dashboard*
	```

4. Access the Dashboard in a browser:

Website: `http://<node-IP>:<NodePort>`
Use the **authentication token** obtained earlier.
    
## Step 6: Confirm Everything is Working

```
kubectl get pods -n kubernetes-dashboard
kubectl get services -n kubernetes-dashboard
```

Check logs if any issues occur:
```
kubectl logs -n kubernetes-dashboard -l k8s-app=kubernetes-dashboard
```
