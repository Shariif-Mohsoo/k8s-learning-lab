# Kubernetes Configuration Management

## Separating Application Settings from Deployment Logic

---

## Introduction

When deploying an application using :contentReference[oaicite:0]{index=0}, it is common for beginners to place all configuration and deployment details into a single file. Initially, this approach may seem simple and effective. However, as the application grows, this structure becomes difficult to manage.

This document explains why separating configuration from deployment is important and how Kubernetes provides built-in solutions to handle this cleanly.

---

## Problem Statement

Consider a deployment file that contains all of the following:

- Database username
- Database password
- Host and port
- Number of replicas
- Storage configuration

If a small change is required, such as updating a database password, the entire deployment file must be edited and reapplied. This creates unnecessary complexity.

As the system grows:

- The number of environment variables increases
- Multiple services are added
- The deployment file becomes difficult to read and maintain

This approach leads to tightly coupled configuration and deployment logic.

---

## Core Principle

Kubernetes follows a clear design principle:

**Application configuration should be separated from deployment logic.**

---

## Conceptual Separation

### Application Configuration

These are values required by the application:

- Database URL
- Username
- Password
- API keys

These values may change frequently.

---

### Deployment Details

These define how the application runs:

- Number of replicas
- Container image
- Volumes
- Networking

These values usually remain stable.

---

## Traditional Approach (Not Recommended)

In the traditional approach, everything is defined in a single deployment file.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: app
          image: node-app
          env:
            - name: DB_USER
              value: admin
            - name: DB_PASSWORD
              value: password123
```

### Issues with This Approach

- Sensitive data is exposed
- Configuration is hardcoded
- Any small change requires redeployment
- Poor maintainability

---

### Kubernetes Solution

Kubernetes introduces two separate objects:

ConfigMap (for non-sensitive data)
Secret (for sensitive data)

### ConfigMap

- A ConfigMap is used to store non-sensitive configuration data in key-value form.

Example: ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DB_HOST: localhost
  DB_PORT: "5432"
  SERVICE_URL: http://api-service
```

### Explanation

- Stores configuration in plain text
- Easy to update without modifying deployment
- Suitable for general settings

---

### Secret

A Secret is used to store sensitive information such as passwords and API keys.

Example: Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  DB_USER: YWRtaW4=
  DB_PASSWORD: cGFzc3dvcmQxMjM=
```

### Explanation

- Values are encoded using Base64
- Kubernetes automatically decodes them before use
- Provides a basic level of protection for sensitive data

---

### Base64 Encoding

Secrets require Base64 encoding to safely store data.

Example Encoding
echo -n "password123" | base64
Output
cGFzc3dvcmQxMjM=

Kubernetes will decode this value when it is used inside the application.

---

### Using ConfigMap and Secret in Deployment

The deployment file becomes cleaner by referencing external configuration.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  template:
    spec:
      containers:
        - name: app
          image: node-app
          env:
            - name: DB_HOST
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: DB_HOST
            - name: DB_PORT
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: DB_PORT
            - name: DB_USER
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: DB_USER
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: DB_PASSWORD
```

### Explanation

- No hardcoded configuration values
- Deployment only defines how the application runs
- Configuration is dynamically injected

---

### Important Rule

ConfigMap and Secret must exist in the same namespace as the Deployment.

If they are not in the same namespace:

The Deployment cannot access them
The application may fail to start
Resource Cleanup Before Refactoring

Before restructuring the system, it is recommended to remove existing resources:

Deployments
StatefulSets
Persistent Volume Claims

This helps:

Avoid configuration conflicts
Ensure correct resource initialization
Simplify debugging
System Dependency Flow

A typical application workflow follows this order:

Database starts
Backend API connects to the database
Frontend connects to the API

Maintaining this order is important for system stability.

Benefits of This Approach
Improved code organization
Easier configuration updates
Enhanced security for sensitive data
Reduced risk of errors
Better scalability

---

### Summary

### ConfigMap:

Stores non-sensitive configuration
Uses plain text

### Secret:

Stores sensitive data
Uses Base64 encoding

### Deployment:

Manages application execution
References ConfigMap and Secret

### Key Takeaway

Deployment is responsible for running the application, ConfigMap provides configuration data, and Secret ensures that sensitive information is handled securely.
