# Reto 4 - Arquitectura Cloud de Microservicios Segura con Terraform

## Descripción

Este proyecto implementa una arquitectura cloud segura, escalable y observable utilizando Terraform sobre AWS. La solución está basada en microservicios desplegados en Kubernetes (Amazon EKS), protegidos mediante autenticación JWT y expuestos a través de API Gateway.

La infraestructura cumple con los requisitos del reto:

* Infraestructura como código (Terraform)
* Arquitectura de microservicios
* Contenedores Docker
* Autenticación JWT
* API Gateway con validación de tokens
* Alta disponibilidad Multi-AZ
* Observabilidad y monitoreo
* Escalabilidad horizontal
* Base de datos gestionada
* Documentación y automatización de despliegue

---

# Arquitectura

## Componentes Cloud

* Amazon VPC
* Public Subnets (2 AZ)
* Private Subnets (2 AZ)
* API Gateway
* AWS WAF
* Amazon EKS
* Amazon Aurora / RDS
* Amazon ElastiCache Redis
* Amazon SQS
* Amazon S3
* Amazon CloudWatch
* AWS X-Ray
* IAM Roles
* Security Groups
* Amazon ECR

## Microservicios

| Servicio             | Responsabilidad                   |
| -------------------- | --------------------------------- |
| Auth Service         | Autenticación y generación de JWT |
| User Service         | Gestión de usuarios               |
| Product Service      | Gestión de productos              |
| Order Service        | Gestión de órdenes                |
| Payment Service      | Procesamiento de pagos            |
| Notification Service | Envío de notificaciones           |

---

# Diagrama de Arquitectura

Consultar:

```text
docs/architecture.png
docs/architecture.drawio
```

---

# Estructura del Proyecto

```text
.
├── terraform/
│   ├── modules/
│   ├── environments/
│   └── main.tf
│
├── microservices/
│   ├── auth-service/
│   ├── user-service/
│   ├── product-service/
│   ├── order-service/
│   ├── payment-service/
│   └── notification-service/
│
├── helm/
│
├── kubernetes/
│
├── postman/
│
├── docs/
│
├── evidence/
│
└── README.md
```

---

# Seguridad

La plataforma implementa múltiples capas de seguridad:

* JWT Authentication
* API Gateway Authorizer
* AWS WAF
* IAM Roles
* Security Groups
* Private Subnets
* Encryption at Rest
* TLS/HTTPS

---

# Flujo de Autenticación

1. Usuario realiza login.
2. Auth Service valida credenciales.
3. Auth Service genera JWT.
4. Cliente envía token Bearer.
5. API Gateway valida JWT.
6. Solicitud es enviada al microservicio correspondiente.

---

# Escalabilidad

La plataforma soporta escalabilidad horizontal mediante:

* Amazon EKS
* Horizontal Pod Autoscaler (HPA)
* Auto Scaling Nodes
* SQS para desacoplamiento de procesos

---

# Observabilidad

## Logs

* CloudWatch Logs

## Métricas

* CPU
* Memoria
* Latencia
* Requests
* Errores

## Alarmas

* High CPU
* High Memory
* API Errors
* RDS CPU Utilization

## Trazabilidad

* AWS X-Ray

---

# Prerrequisitos

## Software

* Terraform >= 1.6
* AWS CLI
* kubectl
* Helm
* Docker

## Credenciales AWS

Configurar:

```bash
aws configure
```

---

# Despliegue de Infraestructura

## Inicializar Terraform

```bash
cd terraform

terraform init
```

## Validar

```bash
terraform validate
```

## Plan

```bash
terraform plan
```

## Aplicar

```bash
terraform apply
```

---

# Despliegue de Aplicaciones

## Construir imágenes

```bash
docker build -t auth-service .
docker build -t user-service .
docker build -t product-service .
docker build -t order-service .
docker build -t payment-service .
docker build -t notification-service .
```

## Publicar en ECR

```bash
aws ecr get-login-password
```

## Instalar Helm

```bash
helm upgrade --install microservices ./helm
```

---

# Destrucción de Recursos

Para eliminar completamente la infraestructura:

```bash
terraform destroy
```

---

# Endpoints

## Auth Service

```http
POST /auth/login
POST /auth/register
```

## User Service

```http
GET /users
GET /users/{id}
POST /users
PUT /users/{id}
DELETE /users/{id}
```

## Product Service

```http
GET /products
POST /products
PUT /products/{id}
DELETE /products/{id}
```

## Order Service

```http
GET /orders
POST /orders
```

## Payment Service

```http
POST /payments
```

## Notification Service

```http
POST /notifications
```

---

# API Testing

La colección Postman se encuentra en:

```text
postman/
```

Importar la colección y configurar:

```text
BASE_URL
JWT_TOKEN
```

---

# Evidencias

Consultar:

```text
evidence/
```

Incluye:

* Terraform Apply
* EKS Running
* Pods Running
* JWT Authentication
* Postman Tests
* CloudWatch Dashboards
* CloudWatch Alarms

---

# Pipeline CI/CD

GitHub Actions automatiza:

1. Terraform Validate
2. Unit Tests
3. Build Docker Images
4. Push ECR
5. Terraform Plan
6. Terraform Apply
7. Helm Deploy

---

# Configuración GitHub Actions

Crear el siguiente Secret:

```text
AWS_ACCOUNT_ID
```

Ruta:

```text
GitHub
→ Settings
→ Secrets and Variables
→ Actions
```

Consultar:

```text
docs/GITHUB_SETUP.md
```

---

# Autor

Proyecto desarrollado para el:

**Reto 4 - Construcción en Terraform de una Arquitectura Cloud de Microservicios Segura**
