# 🧸 Easy README – Health Checker Container (Sidecar Container)

## 🌟 What Is This Part of Code?

This section tells Kubernetes:

> “Along with the main app container, also run a helper container that checks the app’s health.”

Before, we only had the main app container inside the Pod.  
Now, we are adding a **second container inside the same Pod** 📦🩺

---

```yaml
- name: grade-submission-portal-health-checker
  image: rslim087/kubernetes-course-grade-submission-portal-health-checker
  resources:
    requests:
      memory: "128Mi"
      cpu: "200m"
    limits:
      memory: "128Mi"
```

---

🏫 Step-by-Step Explanation
🧩 1. - name: grade-submission-portal-health-checker
Simple Meaning:

This is the name of the second container inside the Pod.

Think like:

First container = Main App 🚀

Second container = Health Checker 🩺

Both live together inside the same Pod (same box) 📦

🖼️ 2. image: rslim087/kubernetes-course-grade-submission-portal-health-checker
Very Important Line ⭐
Simple Meaning:

This tells Kubernetes:

“Download and run this health checker container.”

Real life example:
Image = Pre-built small app that monitors your main app, like a helper or sidekick 🦸

Kubernetes will:

Go to Docker Hub

Pull this image

Run it alongside the main app container

⚡ 3. resources: (Power Control for Health Checker)

This defines how much CPU & Memory the container can use.
Every container inside a Pod consumes resources, so we must define limits and requests.

🧠 4. requests: (Minimum Power Needed)

```yaml
requests: memory: "128Mi";
cpu: "200m";
```

Simple Meaning:

Kubernetes will reserve at least this much memory and CPU for the container.

128Mi → 128 MB RAM 🧠

200m → 0.2 CPU core ⚡

Real life example:

“This helper container needs a small desk and some energy to do its job.”

🚧 5. limits: (Maximum Memory Allowed)

```yaml
limits:
memory: "128Mi"
```

Simple Meaning:

This container cannot use more than 128MB of RAM.

Real life example:

Like giving a strict budget: “You can only spend this much memory, no more.”

---

❓ Important Question:
Why do we need two containers in a single Pod?
Simple Answer:

A Pod is like a single box 📦.

Main container = The main app

Sidecar container = Helper (health checker, logger, monitor)

Reasons for two containers in one Pod:

Health checks – monitor main app and fix/restart if needed

Logging/Monitoring – collect logs or metrics

Sidecar pattern – common DevOps practice for scalable and production-ready apps

Both containers share the same network, storage, and lifecycle because they live inside one Pod.
This makes communication easy and efficient.
