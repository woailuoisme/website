---
sidebar_position: 2
sidebar_label: Kubernetes 部署
title: Kubernetes 部署
description: 使用 Kubernetes 部署应用
---

# Kubernetes 部署

本指南介绍如何在 Kubernetes 集群中部署应用。

## 前置要求

- Kubernetes 1.20 或更高版本
- kubectl 命令行工具
- 访问 Kubernetes 集群的权限

## 基础部署

### Deployment 配置

创建 `deployment.yaml`：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: docs-deployment
  labels:
    app: docs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: docs
  template:
    metadata:
      labels:
        app: docs
    spec:
      containers:
      - name: docs
        image: my-docs:latest
        ports:
        - containerPort: 3000
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
        livenessProbe:
          httpGet:
            path: /
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
```

### Service 配置

创建 `service.yaml`：

```yaml
apiVersion: v1
kind: Service
metadata:
  name: docs-service
spec:
  selector:
    app: docs
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

### 部署应用

```bash
# 应用配置
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# 查看部署状态
kubectl get deployments
kubectl get pods
kubectl get services

# 查看详细信息
kubectl describe deployment docs-deployment
```

## Ingress 配置

创建 `ingress.yaml`：

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: docs-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - docs.example.com
    secretName: docs-tls
  rules:
  - host: docs.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: docs-service
            port:
              number: 80
```

应用 Ingress：

```bash
kubectl apply -f ingress.yaml
```

## ConfigMap 配置

创建 `configmap.yaml`：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: docs-config
data:
  API_URL: "https://api.example.com"
  NODE_ENV: "production"
```

在 Deployment 中使用 ConfigMap：

```yaml
spec:
  containers:
  - name: docs
    envFrom:
    - configMapRef:
        name: docs-config
```

## Secret 管理

创建 Secret：

```bash
kubectl create secret generic docs-secret \
  --from-literal=api-key=your-api-key
```

在 Deployment 中使用 Secret：

```yaml
spec:
  containers:
  - name: docs
    env:
    - name: API_KEY
      valueFrom:
        secretKeyRef:
          name: docs-secret
          key: api-key
```

## 水平自动扩缩容

创建 `hpa.yaml`：

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: docs-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: docs-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

应用 HPA：

```bash
kubectl apply -f hpa.yaml
kubectl get hpa
```

## 使用 Helm

### 创建 Helm Chart

```bash
helm create docs-chart
```

### values.yaml 配置

```yaml
replicaCount: 3

image:
  repository: my-docs
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: LoadBalancer
  port: 80

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: docs.example.com
      paths:
        - path: /
          pathType: Prefix

resources:
  limits:
    cpu: 200m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi

autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70
```

### 部署 Helm Chart

```bash
# 安装
helm install docs ./docs-chart

# 升级
helm upgrade docs ./docs-chart

# 卸载
helm uninstall docs

# 查看状态
helm status docs
```

## 滚动更新

```bash
# 更新镜像
kubectl set image deployment/docs-deployment docs=my-docs:v2

# 查看滚动更新状态
kubectl rollout status deployment/docs-deployment

# 回滚到上一个版本
kubectl rollout undo deployment/docs-deployment

# 查看历史版本
kubectl rollout history deployment/docs-deployment
```

## 监控和日志

### 查看日志

```bash
# 查看 Pod 日志
kubectl logs -f deployment/docs-deployment

# 查看特定 Pod 日志
kubectl logs -f <pod-name>

# 查看前一个容器的日志
kubectl logs <pod-name> --previous
```

### 资源监控

```bash
# 查看资源使用情况
kubectl top nodes
kubectl top pods

# 查看 Pod 详情
kubectl describe pod <pod-name>
```

## 故障排查

### Pod 无法启动

```bash
# 查看 Pod 状态
kubectl get pods
kubectl describe pod <pod-name>

# 查看事件
kubectl get events --sort-by=.metadata.creationTimestamp

# 进入容器调试
kubectl exec -it <pod-name> -- sh
```

### 服务无法访问

```bash
# 检查 Service
kubectl get svc
kubectl describe svc docs-service

# 检查 Endpoints
kubectl get endpoints docs-service

# 端口转发测试
kubectl port-forward svc/docs-service 8080:80
```

## 下一步

- 查看 [云平台部署](./03-cloud-platforms.md)
- 了解 [Docker 部署](./01-docker.md)
