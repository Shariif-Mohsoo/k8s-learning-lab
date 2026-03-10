# Kubernetes Service Configuration – Grade Submission System

## Project Overview

This configuration defines two services in **Kubernetes** to expose two applications:

1. **Grade Submission Portal** – the frontend application used by users.
2. **Grade Submission API** – the backend API used by the portal.

The services allow communication between applications running inside Kubernetes pods.

---

# 1. Service Configuration Code

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grade-submission-portal
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: grade-submission-portal
  ports:
  - port: 5001
    targetPort: 5001
    nodePort: 32000
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grade-submission-api
spec:
  type: ClusterIP
  selector:
    app.kubernetes.io/instance: grade-submission-api
  ports:
  - port: 3000
    targetPort: 3000
```

---

# 2. Line-by-Line Explanation (Beginner Friendly)

## Line 1

```yaml
apiVersion: v1
```

**Meaning**

This tells Kubernetes which API version is used for creating this resource.

Think of it as:

```
Instruction version used by Kubernetes
```

---

## Line 2

```yaml
kind: Service
```

**Meaning**

This tells Kubernetes that we are creating a **Service object**.

A service is used to expose pods and allow communication.

Basic flow:

```
User → Service → Pod
```

---

## Line 3-5

```yaml
metadata:
  name: grade-submission-portal
```

**Meaning**

Metadata contains information about the service.

Here we give the service a name:

```
grade-submission-portal
```

This name is important because other applications can use it to communicate.

---

## Line 6-7

```yaml
spec:
  type: NodePort
```

**Meaning**

The `spec` section describes how the service behaves.

`NodePort` means:

```
Expose this service outside the Kubernetes cluster
```

Users can access it using:

```
NodeIP : NodePort
```

Example:

```
192.168.49.2:32000
```

---

## Line 8-9

```yaml
selector:
  app.kubernetes.io/name: grade-submission-portal
```

**Meaning**

The selector tells Kubernetes:

```
Which pods belong to this service
```

Kubernetes searches for pods with the same label.

Example pod label:

```
app.kubernetes.io/name: grade-submission-portal
```

Then traffic is sent to those pods.

---

## Line 10-14

```yaml
ports:
- port: 5001
  targetPort: 5001
  nodePort: 32000
```

Explanation of each field:

### port

```
port: 5001
```

The port exposed **inside the cluster**.

---

### targetPort

```
targetPort: 5001
```

The port on which the **container/pod application is running**.

Example:

```
Portal app running on port 5001
```

---

### nodePort

```
nodePort: 32000
```

The port opened on the **Kubernetes node**.

So users access the portal like:

```
NodeIP:32000
```

---

# Second Service – API

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grade-submission-api
```

This creates a service named:

```
grade-submission-api
```

---

## Service Type

```yaml
type: ClusterIP
```

This means:

```
The service is only accessible inside the Kubernetes cluster.
```

External users cannot access it directly.

Only internal pods can communicate with it.

---

## Selector

```yaml
selector:
  app.kubernetes.io/instance: grade-submission-api
```

This connects the service with pods having this label.

Traffic goes to those pods automatically.

---

## Ports

```yaml
ports:
- port: 3000
  targetPort: 3000
```

Meaning:

| Field      | Purpose              |
| ---------- | -------------------- |
| port       | Service port         |
| targetPort | Pod application port |

Example flow:

```
Portal → grade-submission-api:3000 → API Pod
```

---

# 3. Service Discovery (Important Concept)

Service discovery means:

```
How applications find each other inside Kubernetes
```

In Kubernetes every service automatically gets a **DNS name**.

Example:

```
grade-submission-api.default.svc.cluster.local
```

But most apps simply use:

```
grade-submission-api
```

Example request from portal:

```
http://grade-submission-api:3000
```

Flow:

```
Portal Pod
   ↓
Kubernetes DNS
   ↓
API Service
   ↓
API Pod
```

This automatic discovery is handled by **Kubernetes DNS**.

---

# 4. Difference Between ClusterIP and NodePort

| Feature       | ClusterIP         | NodePort        |
| ------------- | ----------------- | --------------- |
| Access        | Internal only     | External access |
| Used by       | Internal services | User access     |
| Example use   | API, database     | Frontend apps   |
| Access format | service-name      | NodeIP:Port     |

---

## ClusterIP Example

```
Portal → grade-submission-api
```

Only internal communication.

---

## NodePort Example

User opens browser:

```
http://NodeIP:32000
```

This reaches the portal service.

---

# 5. Complete System Flow

```
User
 ↓
NodeIP:32000
 ↓
grade-submission-portal (NodePort)
 ↓
Portal Pod
 ↓
grade-submission-api (ClusterIP)
 ↓
API Pod
```

---

# 6. Key Takeaways

• Services provide stable access to pods
• NodePort exposes apps to external users
• ClusterIP allows internal communication
• Service discovery lets apps communicate using service names

---

# 7. Useful Commands

View services:

```
kubectl get svc
```

Describe service:

```
kubectl describe svc grade-submission-portal
```

Delete service:

```
kubectl delete svc grade-submission-portal
```

---

# Conclusion

This setup creates two services:

1. **grade-submission-portal (NodePort)**
   Used by users to access the application.

2. **grade-submission-api (ClusterIP)**
   Used internally by the portal to communicate with the backend API.

This architecture is commonly used in microservice-based Kubernetes applications.
