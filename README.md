# 🍽️ EurekaEats – Cloud-Native Food Ordering Microservice

![Java](https://img.shields.io/badge/Java-21-blue.svg)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.5-brightgreen.svg)
![MongoDB](https://img.shields.io/badge/MongoDB-7.0-green.svg)
![Docker](https://img.shields.io/badge/Docker-24.0-blue.svg)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28-blue.svg)
![AWS EKS](https://img.shields.io/badge/AWS-EKS-orange.svg)
![Netflix Eureka](https://img.shields.io/badge/Service%20Discovery-Eureka-yellow.svg)

## Overview

Order Service is a production-ready cloud-native microservice built using Spring Boot and MongoDB.  
It handles food ordering operations, registers with Eureka for service discovery, and is containerized with Docker and deployed on AWS EKS for scalability and high availability.

---
#Note:- The the link deployed app is disabled because aws services cost is very high.
---

This project demonstrates:

- Microservice architecture
- Service discovery
- Cloud deployment
- Container orchestration
- RESTful API design
- Kubernetes production configuration
---

# 📦 Prerequisites

- JDK 21
- Maven 3.8+
- Docker
- kubectl
- AWS CLI configured
- MongoDB Atlas account
- Eureka Server

---
# 📁 Project Structure

```
order-service/
├── src/main/java/com/codedecode/order/
│   ├── controller/
│   ├── service/
│   ├── repo/
│   ├── entity/
│   ├── dto/
│   ├── mapper/
│   └── OrderMsApplication.java
│
├── src/main/resources/
│   ├── application.yml
│   ├── application-local.yml
│   ├── application-dev.yml
│   ├── static/
│   ├── order-deployment.yaml
│   └── order-service.yaml
│
├── Dockerfile
├── pom.xml
└── README.md
```
---

# 🏗 Architecture

## High-Level Architecture

```
                        ┌─────────────────────────┐
                        │        End User         │
                        │        (Browser)        │
                        └────────────┬────────────┘
                                     │
                                     ▼
                        ┌─────────────────────────┐
                        │   AWS Load Balancer     │
                        │  (Kubernetes Service)   │
                        └────────────┬────────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │        Kubernetes Cluster       │
                    │            (AWS EKS)            │
                    │                                 │
                    │   ┌─────────────────────────┐   │
                    │   │     Order Service Pod   │   │
                    │   │  (Spring Boot App)      │   │
                    │   └────────────┬────────────┘   │
                    │                │                │
                    │                ▼                │
                    │   ┌─────────────────────────┐   │
                    │   │      Eureka Server      │   │
                    │   │  (Service Discovery)    │   │
                    │   └────────────┬────────────┘   │
                    └────────────────│────────────────┘
                                     │
                                     ▼
                        ┌─────────────────────────┐
                        │      MongoDB Atlas      │
                        │   (Cloud NoSQL DB)      │
                        └─────────────────────────┘
```

---

## Architecture Breakdown

| Layer | Component | Responsibility |
|-------|-----------|---------------|
| Client Layer | Browser UI | Displays menu, cart, and order interface |
| API Layer | Spring Boot REST Controllers | Handles HTTP requests |
| Business Layer | Service Classes | Order processing logic |
| Persistence Layer | MongoDB Repository | Data storage |
| Service Discovery | Netflix Eureka | Dynamic microservice registration |
| Container Layer | Docker | Application packaging |
| Orchestration | Kubernetes (EKS) | Scaling, rolling updates, self-healing |
| Cloud Infrastructure | AWS | Hosting and networking |

---

## Request Flow

1. Client sends request to LoadBalancer.
2. Kubernetes routes request to Order Service Pod.
3. Order Service processes request.
4. If needed, discovers other services via Eureka.
5. Stores or retrieves data from MongoDB Atlas.
6. Response returned to client.

---

## 🛠 Tech Stack
| Category              | Technology Stack                                                               |
|-----------------------|--------------------------------------------------------------------------------|
| Backend               | Java 21, Spring Boot 3.2.5, Spring Cloud (Eureka Client), Spring Data MongoDB  |
| Frontend              | HTML5, CSS3, Bootstrap 5, JavaScript                                           |
| Database              | MongoDB Atlas                                                                  |
| Service Discovery     | Netflix Eureka                                                                 |
| Build Tool            | Maven                                                                          |
| Containerization      | Docker                                                                         |
| Container Registry    | Amazon ECR                                                                     |
| Orchestration         | Kubernetes (EKS)                                                               |
| Cloud Infrastructure  | AWS (EKS, ECR, IAM, VPC, LoadBalancer)                                         |
| Version Control       | Git, GitHub                                                                    |
---

# 🚀 Local Development Setup

## Clone Repository

```bash
git clone https://github.com/your-username/order-service.git
cd order-service
```

## Configure MongoDB

Edit:

```
src/main/resources/application-local.yml
```

```yaml
spring:
  data:
    mongodb:
      uri: mongodb+srv://<username>:<password>@cluster.mongodb.net/orderdb
```

## Build Project

```bash
./mvnw clean package
```

---

# ⚙ Configuration Profiles

## ⚙ Configuration Profiles

| Profile | Purpose            | Key Settings                                                     |
|---------|--------------------|------------------------------------------------------------------|
| local   | Local development  | MongoDB URI, Eureka disabled                                     |
| dev     | EKS deployment     | Eureka enabled, cluster service URL configured                   |

---

# 🏃 Run Application Locally

```bash
java -jar target/order-0.0.1-SNAPSHOT.jar --spring.profiles.active=local
```

Access:

```
http://localhost:8081
```

---

# ☁ Docker & Amazon ECR

## Build JAR File

```bash
mvn clean package 
```

## Authenticate Docker

```bash
aws ecr get-login-password --region ap-south-1 \
| docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-south-1.amazonaws.com
```

## Build Docker Image

```bash
docker build -t order-service:latest .
```

## Tag Image

```bash
docker tag order-service:latest <account-id>.dkr.ecr.ap-south-1.amazonaws.com/order-service:latest
```

## Push Image

```bash
docker push <account-id>.dkr.ecr.ap-south-1.amazonaws.com/order-service:latest
```

---

# 🚢 Deploy to EKS

## Apply Kubernetes Manifests

```bash
kubectl apply -f src/main/resources/order-deployment.yaml
kubectl apply -f src/main/resources/order-service.yaml
```

## Restart Deployment

```bash
kubectl rollout restart deployment order-service
```

## Verify Deployment

```bash
kubectl get pods
kubectl get svc order-service
kubectl logs -f deployment/order-service
```

Access using:

```
http://<EXTERNAL-IP>:8081
```

---

# 📡 API Endpoints

## 📡 API Endpoints

| Method | Endpoint           | Description              | Request Body           | Response                  |
|--------|--------------------|--------------------------|------------------------|---------------------------|
| POST   | /order/saveOrder   | Place a new order        | OrderDTOFromFE (JSON)  | OrderDTO (201 Created)    |
| POST   | /food/items        | Fetch all food items     | Empty JSON {}          | List of FoodItemDTO (200) |
| POST   | /food/add          | Add new food item        | FoodItemDTO (JSON)     | FoodItemDTO (201 Created) |

---

## Example: Place Order

```bash
curl -X POST http://localhost:8081/order/saveOrder \
-H "Content-Type: application/json" \
-d '{
  "foodItemsList": [
    {
      "id": "69a3f72ee2c6a70950fe7665",
      "itemName": "Burger",
      "price": 150,
      "quantity": 2
    }
  ],
  "userId": 1
}'
```

---

# 🖥 Frontend UI

- Dynamic food menu from MongoDB
- Add-to-cart functionality
- Cart summary model
- Order placement
- Responsive Bootstrap layout

Static resources:

```
src/main/resources/static/
```

---

# ✨ Features

- Dynamic menu from database
- MongoDB persistence
- Eureka-based service discovery
- Docker containerization
- Kubernetes deployment
- Load-balanced scaling
- Scalable pods
- Environment-based configuration

---

# 👤 Author

Omkar Dhenge

---

If this project helped you, consider giving it a star.
