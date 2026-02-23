# 🧸 Easy README – Container Spec (Grade Submission Portal)

## 🌟 What Is This Part of Code?

This section tells Kubernetes:

> “What should be inside the Pod box and how much power (CPU & Memory) it can use.”

Before, we only created the box (Pod).
Now, we are putting the **real app inside the box** 📦🚀

---

spec:
containers:

- name: grade-submission-portal
  image: rslim087/kubernetes-course-grade-submission-portal
  resources:
  requests:
  memory: "128Mi"
  cpu: "200m"
  limits:
  memory: "128Mi"
  ports:
  - containerPort: 5001

# 🏫 Step-by-Step Explanation

## 📘 1. `spec:`

### Simple Meaning:

`spec` means:

> The detailed plan of the Pod.

Real life example:
Like instructions inside a school project file:

- What to include
- How it should work
- What resources it needs

So this section explains how the Pod should run.

---

## 🧩 2. `containers:`

### Simple Meaning:

This tells Kubernetes:

> What container (app) should run inside the Pod.

Remember:

- Pod = Box 📦
- Container = The actual app inside the box

One Pod can have one or more containers.

---

## 📛 3. `- name: grade-submission-portal`

### Simple Meaning:

This is the name of the container (the app inside the Pod).

Think like:

- Pod Name = Box Name
- Container Name = App Name inside the box

Here the app name is:

> grade-submission-portal

---

## 🖼️ 4. `image: rslim087/kubernetes-course-grade-submission-portal`

### Very Important Line ⭐

### Simple Meaning:

This tells Kubernetes:

> Which app image to download and run.

Real life example:
Image = Ready-made app package
Like downloading a game from Play Store and running it.

So Kubernetes will:

1. Go to Docker Hub
2. Download this image
3. Run it inside the container

---

# ⚡ 5. `resources:` (Power Control Section)

This part controls how much **computer power** your app can use.

Real life example:
Imagine your app is a student using:

- RAM = Memory (brain space 🧠)
- CPU = Thinking speed ⚡

---

## 🧠 6. `requests:` (Minimum Resources)

### Simple Meaning:

Requests = Minimum power your app needs to start safely.

```yaml
requests:
  memory: "128Mi"
  cpu: "200m"
```

This tells Kubernetes:

> “Please reserve at least this much power for my app.”

### Breakdown:

- `128Mi` = 128 MB RAM
- `200m` CPU = 0.2 CPU (20% of one CPU core)

School example:
Like saying:

> “I need at least one desk and one book to study.”

---

## 🚧 7. `limits:` (Maximum Resources)

```yaml
limits:
  memory: "128Mi"
```

### Simple Meaning:

Limits = Maximum power the app is allowed to use.

Here it means:

> App cannot use more than 128MB memory.

Real life example:
Like a rule:

> “You can use only one notebook, not more.”

This prevents the app from crashing the system by using too much RAM.

---

## 🌐 8. `ports:`

```yaml
ports:
  - containerPort: 5001
```

### Simple Meaning:

This tells Kubernetes:

> The app is running on port 5001 inside the container.

Real life example:
Port = Door 🚪 of the app
Users enter through this door to use the app.

So:

- Port 5001 = Entry door of your Grade Submission Portal

---

# ❓ Important Question 1:

# Why we did NOT specify CPU limit?

### Simple Answer (Student Level):

Because sometimes we want the app to:

> Use extra CPU if available for better performance.

If we set a CPU limit:

- App cannot use more CPU even if system is free
- This can make the app slow 😢

So developers often:

- Set CPU request ✅
- Skip CPU limit ⚡ (for flexibility)

---

# 🧠 Technical but Easy Truth:

- Memory limit is important (to prevent crashes)
- CPU limit is optional (CPU is compressible resource)

Memory = Strict resource
CPU = Flexible resource

---

# ❓ Important Question 2:

# How to Decide (Guess) CPU & Memory Requests?

This is a VERY real-world DevOps skill 🔥

### 🪜 Step-by-Step Simple Method:

## 🥇 Method 1 (Beginner – Recommended for You)

Start small and safe:

- Memory: 128Mi – 256Mi (for small apps)
- CPU: 100m – 300m (for simple web apps)

Since your project is a frontend portal:

> 128Mi RAM + 200m CPU = Perfect beginner choice ✅

---

## 🥈 Method 2 (Smart Method – Observe Usage)

Run the app and check usage:

```bash
kubectl top pod
```

Then see:

- How much memory it uses
- How much CPU it uses

Example:
If app uses:

- 90Mi memory → set request ~128Mi
- 150m CPU → set request ~200m

---

## 🥉 Method 3 (Production Method – Professional Way)

Use monitoring tools like:

- Metrics Server
- Prometheus
- Grafana

They show real resource usage graphs 📊
Then you set accurate requests & limits.

---

# 🎓 Final Super Easy Summary

This code section is saying:

> “Dear Kubernetes 👋
> Inside my Pod, run a container named grade-submission-portal
> Use this app image from Docker
> Reserve minimum 128MB RAM and 0.2 CPU
> Do not allow memory above 128MB
> And run the app on port 5001.”

---

# 🚀 Why This Is Important for You (Especially FYP & Cloud Apps)

By understanding this:

- You can deploy AI models on Kubernetes
- Control server costs 💰
- Prevent app crashes
- Build scalable real-world applications

This is a **core DevOps + Cloud skill** that very few students learn early — and you are learning it now 👑
