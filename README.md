# Kubernetes Test Project

## 📖 About the Project
This project was created to learn and demonstrate application management, configuration, and deployment processes on Kubernetes. The project can be used to showcase Kubernetes knowledge and skills during job interviews.

The following tasks were completed as part of this project:
- **Namespace Management:** Creation of `test` and `production` namespaces.
- **RBAC (Role-Based Access Control):** Authorization for user groups.
- **StatefulSet and Deployment:** Deployment of MongoDB StatefulSet and WordPress-MySQL applications.
- **Ingress Management:** Exposing applications to the outside world using Ingress resources.
- **Service Account Usage:** Creating a custom Service Account for accessing the Kubernetes API.
- **Log Management:** Collecting logs using Fluentd DaemonSet.

---

## 🚀 Features and Tasks

### 1. **Namespace Management**
- `test` and `production` namespaces were created.
- Namespaces were created using the following commands instead of YAML files:
  ```bash
  kubectl create namespace test
  kubectl create namespace production
  ```

### 2. **RBAC (Role-Based Access Control)**
- Different authorizations were created for **Junior** and **Senior** groups.
- A **Role** and **RoleBinding** with full permissions were created for the `senior` group.
- YAML files:
  - `rbac/junior.yaml`
  - `rbac/senior.yaml`

### 3. **StatefulSet and Deployment**
- **MongoDB StatefulSet:**
  - A 2-node MongoDB cluster was created in the `production` namespace.
  - Data was persisted using PersistentVolume and PersistentVolumeClaim.
  - YAML file: `statefulset/mongodb-production.yaml`
- **WordPress and MySQL Deployment:**
  - WordPress and MySQL applications were deployed in both `test` and `production` namespaces.
  - Sensitive information was managed using Kubernetes Secrets.
  - YAML files:
    - `deployments/production-deployment.yaml`
    - `deployments/test-deployment.yaml`

### 4. **Ingress Management**
- Ingress resources were created for WordPress applications:
  - `testblog.example.com` (test namespace)
  - `companyblog.example.com` (production namespace)
- Instead of **LoadBalancer** type services, Minikube Ingress was used. The following entries were added to the host file:
  ```
  127.0.0.1 testblog.example.com
  127.0.0.1 companyblog.example.com
  ```
- YAML files:
  - `ingress/test-ingress.yaml`
  - `ingress/production-ingress.yaml`

### 5. **Service Account Usage**
- A custom **Service Account** was created for accessing the Kubernetes API.
- A pod using this Service Account was deployed, and pod information was successfully retrieved from the API.
- YAML files:
  - `service-account/service-account.yaml`
  - `service-account/cluster-role.yaml`
  - `service-account/test-pod.yaml`

### 6. **Log Management**
- Logs from all nodes were collected using Fluentd DaemonSet.
- YAML file: `deamonset/fluentd.yaml`

---

## 📂 Project Structure
```plaintext
k8s-test-project/
├── deamonset/
│   └── fluentd.yaml
├── deployments/
│   ├── liveness-readiness-update.yaml
│   ├── production-deployment.yaml
│   └── test-deployment.yaml
├── ingress/
│   ├── production-ingress.yaml
│   └── test-ingress.yaml
├── rbac/
│   ├── junior.yaml
│   ├── senior.yaml
│   └── cluster-role.yaml
├── service-account/
│   ├── cluster-role.yaml
│   ├── service-account.yaml
│   └── test-pod.yaml
├── statefulset/
│   └── mongodb-production.yaml
└── logs.txt
```

---

## 🔧 Setup and Execution
### 1. **Start the Kubernetes Cluster**
A cluster with 1 master and 2 worker nodes was started on Minikube:
```bash
minikube start --nodes=3
```

### 2. **Create Namespaces**
Create the namespaces using the following commands:
```bash
kubectl create namespace test
kubectl create namespace production
```

### 3. **Deploy MongoDB StatefulSet**
Deploy the MongoDB StatefulSet using:
```bash
kubectl apply -f statefulset/mongodb-production.yaml
```

### 4. **Deploy WordPress and MySQL**
Deploy the WordPress and MySQL applications using:
```bash
kubectl apply -f deployments/production-deployment.yaml
kubectl apply -f deployments/test-deployment.yaml
```

### 5. **Apply Ingress**
Create the Ingress resources using:
```bash
kubectl apply -f ingress/test-ingress.yaml
kubectl apply -f ingress/production-ingress.yaml
```

### 6. **Test the Service Account**
Deploy the pod using the Service Account and test access to the Kubernetes API:
```bash
kubectl apply -f service-account/test-pod.yaml
kubectl exec -it pod-reader -n kube-system -- /bin/sh
```

---

## 📜 Notes
- This project was designed to demonstrate application management and configuration skills on Kubernetes.
- Since it runs on Minikube, the Minikube Ingress addon must be enabled for Ingress resources to work:
  ```bash
  minikube addons enable ingress
  minikube tunnel
  ```