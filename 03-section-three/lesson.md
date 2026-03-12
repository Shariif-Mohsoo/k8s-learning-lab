# Kubernetes Namespaces – Beginner Guide (Story Style)

## 🎯 Goal of This Section

In this section we learn about **Namespaces in Kubernetes**.
Namespaces help us **organize and manage resources inside a cluster**.

To make this easy, we will understand it through a **simple story**.

---

# 📖 Story: The Big School Building

Imagine a **very big school building**.

Inside the building there are many rooms:

- Room for **Math class**
- Room for **Science class**
- Room for **Computer class**
- Room for **Library**

Each room has its **own students and materials**.

Even though everything is inside **one building**, the rooms help keep things **organized**.

### Kubernetes works the same way.

The **school building** = Kubernetes Cluster
The **rooms** = Namespaces

So namespaces help us **group related resources together**.

---

# 🧠 Understanding the Kubernetes Cluster

A Kubernetes cluster is made of different machines.

There are two main types:

### 1️⃣ Master Nodes

These are like the **school principal**.

They decide:

- Where containers should run
- How resources are managed
- How the system should behave

---

### 2️⃣ Worker Nodes

These are like the **classrooms where work happens**.

Worker nodes actually **run the containers**.

Inside each worker node there are important components:

| Component         | Simple Meaning                  |
| ----------------- | ------------------------------- |
| Container Runtime | Creates and runs containers     |
| Kubelet           | Makes sure pods run correctly   |
| Kube Proxy        | Handles networking between pods |

All these machines work together and make it look like **one system**.

So when developers use Kubernetes, it **feels like one big computer**.

---

# 📦 What is a Namespace?

A **namespace** is a **logical group** inside Kubernetes.

Important thing:

A namespace is **not a physical machine**.

It does **not consume CPU or memory**.

It is simply a way to **organize resources**.

Think of it like:

```
Cluster
 ├── Namespace A
 │    ├── Pods
 │    ├── Services
 │
 ├── Namespace B
 │    ├── Pods
 │    ├── Services
 │
 └── Namespace C
      ├── Pods
      ├── Services
```

---

# 📚 Example Namespaces

In real systems, developers create namespaces for different purposes.

Example:

| Namespace        | What it Contains                |
| ---------------- | ------------------------------- |
| grade-submission | Portal + API resources          |
| logging          | Elasticsearch, Logstash, Kibana |
| monitoring       | Prometheus, Grafana             |

This keeps everything **clean and organized**.

---

# 🔐 Why Namespaces are Important

Namespaces help in **three major ways**.

### 1️⃣ Organization

They group related resources.

Example:

```
grade-submission namespace
   ├── portal pod
   ├── api pod
   ├── services
```

Everything related to that project stays together.

---

### 2️⃣ Security (Access Control)

In large companies, not everyone can access everything.

Namespaces allow **role based access control**.

Example:

Developer working on:

```
grade-submission
```

Can access only:

```
grade-submission namespace
```

But cannot access:

```
monitoring namespace
```

---

### 3️⃣ Resource Limits

Namespaces can limit resources.

Example rule:

```
Grade submission namespace
   Memory: 1GB
   CPU: 1 Core
```

This prevents one project from **using all cluster resources**.

---

# 🔎 Checking Existing Namespaces

Command:

```bash
kubectl get namespaces
```

Example output:

```
default
kube-system
ingress-nginx
```

### Meaning of Each

| Namespace     | Purpose                        |
| ------------- | ------------------------------ |
| default       | Default location for resources |
| kube-system   | Kubernetes internal components |
| ingress-nginx | Ingress controller resources   |

---

# 📦 Where Were Our Pods Running?

Previously we created pods like this:

```
kubectl get pods
```

But we never specified a namespace.

So Kubernetes automatically placed them in:

```
default namespace
```

To check pods inside default namespace:

```bash
kubectl get pods -n default
```

---

# 🧹 Deleting Old Pods

Before moving resources, we remove old pods.

Command:

```bash
kubectl delete pods --all -n default
```

Meaning:

```
Delete all pods inside the default namespace
```

---

# 🆕 Creating Our Own Namespace

Now we create a namespace for our project.

Command:

```bash
kubectl create namespace grade-submission
```

Check again:

```bash
kubectl get namespaces
```

Now you should see:

```
grade-submission
```

---

# 🚀 Deploying Resources in a Namespace

Now we deploy all configuration files.

Command:

```bash
kubectl apply -f . -n grade-submission
```

Meaning:

```
Apply all files in the current folder
inside the grade-submission namespace
```

---

# ⚠️ Possible Error (NodePort Conflict)

Sometimes Kubernetes shows an error.

Example reason:

A service already exists in another namespace using the same port.

Check services in default namespace:

```bash
kubectl get services -n default
```

Delete them if needed:

```bash
kubectl delete service grade-submission-api
kubectl delete service grade-submission-portal -n default
```

---

# 🔄 Deploy Again

Now run deployment again:

```bash
kubectl apply -f . -n grade-submission
```

Now services and pods will be created successfully.

---

# 🔎 Checking Resources in Namespace

Check pods:

```bash
kubectl get pods -n grade-submission
```

Check services:

```bash
kubectl get services -n grade-submission
```

---

# 🧠 Better Method (Recommended)

Instead of specifying namespace in the command every time, we can define it **inside the configuration file**.

Example:

```
metadata:
  name: grade-submission-api
  namespace: grade-submission
```

Now Kubernetes **always deploys it to that namespace**.

Even if we run:

```
kubectl apply -f .
```

It will go directly into:

```
grade-submission namespace
```

---

# 🧹 Deleting Everything from Namespace

To delete everything inside a namespace:

```bash
kubectl delete pods,services --all -n grade-submission
```

Meaning:

```
Delete all pods and services
inside grade-submission namespace
```

---

# 🧠 Final Understanding

Even though Kubernetes clusters run on **many machines**, developers experience it like **one system**.

Namespaces create **logical boundaries** that help us:

- Organize applications
- Control access
- Manage resources
- Keep projects separate

---

# 🧾 Key Commands Summary

| Command                           | Purpose                |
| --------------------------------- | ---------------------- |
| kubectl get namespaces            | View namespaces        |
| kubectl create namespace NAME     | Create namespace       |
| kubectl get pods -n NAME          | View pods in namespace |
| kubectl apply -f . -n NAME        | Deploy resources       |
| kubectl delete pods --all -n NAME | Delete pods            |

---

# 🎉 Conclusion

Namespaces are like **rooms in a big building**.

The building is **Kubernetes Cluster**.

Each room (namespace) keeps **related resources together**, making the system easier to manage, secure, and organize.

Once you understand namespaces, managing large Kubernetes systems becomes **much easier**.
