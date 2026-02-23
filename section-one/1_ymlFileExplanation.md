# 🧸 Easy README – Grade Submission Portal Pod (Kubernetes)

## 🌟 What Is This File?

This file is a small instruction sheet for Kubernetes.

It tells Kubernetes:

> “Please create a Pod (a small box) to run my app called Grade Submission Portal.”

Think of it like giving instructions to a robot manager 🤖 who runs your app on computers.

---

apiVersion: v1
kind: Pod
metadata:
name: grade-submission-portal
labels:
app.kubernetes.io/name: grade-submission
app.kubernetes.io/component: frontend
app.kubernetes.io/instance: grade-submission-portal

# 🏫 Step-by-Step Explanation

## 📘 1. `apiVersion: v1`

### Simple Meaning:

This tells Kubernetes which rule book to follow.

Just like in school:

> “We are using Class 6 syllabus.”

Here:

- `apiVersion` = Rule book
- `v1` = Basic and simple version

---

## 📦 2. `kind: Pod`

### Simple Meaning:

This tells Kubernetes:

> “Please create a Pod.”

### But what is a Pod?

A Pod is like a **small box** 📦 that holds and runs your app.

Real life example:

- Pod = Lunch box
- App = Food inside the lunch box

So Kubernetes makes a box and puts your app inside it.

---

## 🏷️ 3. `metadata`

### Simple Meaning:

Metadata means **information about the Pod**.

Like a school form that contains:

- Name
- Details
- Identification

It helps Kubernetes understand and manage your app easily.

---

## 🧾 4. `name: grade-submission-portal`

### Simple Meaning:

This is the name of your Pod.

So the Pod’s name is:

> grade-submission-portal

Real life example:
Like naming your project:

> “Science Model” or “Math Project”

Now Kubernetes can find your Pod using this name.

---

## 🎯 5. `labels`

### Very Important Concept ⭐

Labels are like **stickers on a box**.

Example stickers:

- Frontend
- Backend
- Database
- App Name

Kubernetes uses these stickers (labels) to:

- Find apps
- Group apps
- Manage apps easily

---

## 🏷️ Label 1:

`app.kubernetes.io/name: grade-submission`

### Simple Meaning:

This sticker tells:

> This app belongs to the Grade Submission system.

Like writing on a file:

> “School Grade System”

---

## 🖥️ Label 2:

`app.kubernetes.io/component: frontend`

### Simple Meaning:

This tells Kubernetes:

> This Pod is the FRONTEND part (the screen users see).

Real life example:

- Frontend = Website screen / UI
- Backend = Hidden logic behind the app

So this Pod is the user interface part of the portal.

---

## 🆔 Label 3:

`app.kubernetes.io/instance: grade-submission-portal`

### Simple Meaning:

This is the unique identity of this app instance.

Like in school:

- Name = Student Name
- Instance = Roll Number (unique)

Even if many apps exist, Kubernetes knows exactly which one this is.

---

# 🎓 Final Super Easy Summary

This whole file is saying:

> “Dear Kubernetes 👋
> Please create one Pod (box)
> Name it ‘grade-submission-portal’
> It is a frontend app
> And it belongs to a Grade Submission system.”

---

# 🚀 Why This Is Useful (Student-Friendly)

By learning this file, you understand:

- How apps run in Kubernetes
- What a Pod is
- How naming and labels work
- How real-world cloud apps are organized

This is the **first step** to deploying real applications like:

- Student portals
- School systems
- Web apps
- AI-powered applications (useful for FYP projects)
