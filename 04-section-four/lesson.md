# Kubernetes Deployments & Replicas – Beginner Guide (Story Style)

## 🎯 Goal of This Section

In this section, we learn:

- What is a **Deployment**
- What are **Replicas**
- How Kubernetes **automatically manages pods**
- How **load balancing works**

We will learn everything through a **simple story** 👇

---

# 📖 Story: The Popular Restaurant 🍔

Imagine you own a **restaurant**.

At first, you have:

```id="a1"
1 chef (1 pod)
```

Everything is fine…

But suddenly your restaurant becomes **very popular** 😱

Now:

- Too many customers
- Orders are slow
- One chef cannot handle everything

---

## ❓ What should you do?

Simple…

👉 Hire more chefs!

```id="a2"
3 chefs (3 pods)
```

Now work becomes faster ✅

---

# 🧠 Kubernetes Solution

In **Kubernetes**, instead of chefs we have:

```id="a3"
Pods (your app instances)
```

And instead of manually creating pods, we use:

```id="a4"
Deployment
```

---

# 📦 What is a Deployment?

A **Deployment** is like a **manager** 👨‍💼

You tell the manager:

```id="a5"
"I want 2 or 3 pods running"
```

And Kubernetes will:

✔ Create them
✔ Monitor them
✔ Replace them if they fail

---

# 🔁 What are Replicas?

Replicas mean:

```id="a6"
Copies of the same pod
```

Example:

```id="a7"
1 app → 2 replicas → 2 identical pods
```

All pods run the **same application**.

---

# ⚙️ What Happens Behind the Scene?

Let’s simplify this:

```id="a8"
Deployment → ReplicaSet → Pods
```

### Step-by-step:

1. You create a **Deployment**
2. Deployment creates a **ReplicaSet**
3. ReplicaSet creates **Pods**

---

## 🧠 Important Idea

ReplicaSet ensures:

```id="a9"
Desired number of pods are ALWAYS running
```

---

# 🔥 Example

You say:

```id="a10"
replicas: 2
```

Kubernetes will make sure:

```id="a11"
2 pods are always running
```

---

# 😱 What if a Pod Dies?

Let’s go back to the restaurant 🍔

One chef suddenly leaves…

What happens?

👉 Manager hires a new chef immediately!

---

Same in Kubernetes:

```bash id="a12"
kubectl delete pod pod-name
```

Kubernetes will:

```id="a13"
Automatically create a new pod
```

So you still have:

```id="a14"
2 running pods
```

This is called:

## ✅ Automated Self-Healing

---

# 🧪 Real Commands Used

### Delete old pods

```bash id="a15"
kubectl delete pods --all -n grade-submission
```

---

### Apply deployment

```bash id="a16"
kubectl apply -f .
```

---

### Check deployments

```bash id="a17"
kubectl get deployments -n grade-submission
```

---

### Check pods

```bash id="a18"
kubectl get pods -n grade-submission
```

---

# 📦 Two Deployments in Our Project

We created:

## 1️⃣ API Deployment

```id="a19"
Replicas: 2
```

Meaning:

```id="a20"
2 API pods running
```

---

## 2️⃣ Portal Deployment

```id="a21"
Replicas: 1
```

Meaning:

```id="a22"
1 frontend pod running
```

---

# 🧠 Why Different Replicas?

Because:

- API handles more work → needs more pods
- Portal is lighter → needs fewer pods

---

# 🔗 Connection with Services

Remember services?

They connect to pods using **labels**.

Example:

```id="a23"
Service → finds pods → sends traffic
```

---

# ⚖️ Load Balancing (Very Important)

Now we have:

```id="a24"
2 API pods
```

When requests come:

Kubernetes will:

```id="a25"
Send request to Pod 1
Send request to Pod 2
Send request to Pod 1
Send request to Pod 2
```

This is called:

## ✅ Load Balancing

---

## 🎯 Why Load Balancing?

Without it:

```id="a26"
1 pod gets all traffic → crash 😵
```

With it:

```id="a27"
Traffic shared → system stable 😎
```

---

# 🌐 Full Flow of System

```id="a28"
User
 ↓
NodePort Service (Portal)
 ↓
Portal Pod
 ↓
ClusterIP Service (API)
 ↓
API Pods (2 replicas)
```

---

# ⚠️ Common Mistake

Deployment created in wrong namespace ❌

Fix:

```yaml id="a29"
metadata:
  name: grade-submission-api
  namespace: grade-submission
```

Always specify namespace!

---

# 🧹 Deleting Deployments

```bash id="a30"
kubectl delete deployments --all -n default
```

---

# 🧠 Final Understanding

| Concept        | Simple Meaning      |
| -------------- | ------------------- |
| Pod            | One app instance    |
| Replica        | Copy of pod         |
| Deployment     | Manager of pods     |
| ReplicaSet     | Maintains pod count |
| Load Balancing | Distributes traffic |

---

# 🎉 Key Benefits

Using Deployment gives you:

✔ Automatic pod creation
✔ Automatic recovery (self-healing)
✔ Load balancing
✔ Easy scaling

---

# 🧾 Summary in One Line

```id="a31"
Deployment = "Keep my app running no matter what"
```

---

# 🏁 Conclusion

Deployments make Kubernetes **powerful and easy**.

Instead of manually managing pods, you just say:

```id="a32"
"I want 2 pods"
```

And Kubernetes handles everything for you:

- Creation
- Monitoring
- Recovery
- Scaling

---

✨ Now you are one step closer to mastering Kubernetes!
