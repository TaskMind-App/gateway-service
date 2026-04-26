# TaskMind - API Gateway Service

## 📝 Overview
The **API Gateway Service** serves as the single entry point for all client applications (UI/Mobile) interacting with the TaskMind ecosystem. It acts as a smart reverse proxy, bridging the gap between external clients and the internal microservice architecture while ensuring security and efficiency.

## 🚀 Key Responsibilities
* **Request Routing:** Dynamically routing incoming HTTP requests to the appropriate downstream microservice using the **Discovery Service** (Service Discovery).
* **Security & Auth Validation:** Centralized validation of JWT tokens to ensure only authorized requests reach the internal domain services.
* **Cross-Cutting Concerns:** Managing CORS policies, Rate Limiting to prevent API abuse, and centralized request logging for monitoring.
* **Abstraction:** Providing a secure boundary that hides the internal network topology from the outside world.

## 🛠 Tech Stack
* **Language:** Java (OpenJDK 17+)
* **Framework:** Spring Cloud Gateway / Spring Boot
* **Service Discovery:** Netflix Eureka / Consul Client
* **Security:** JWT Validation & Spring Security
* **Load Balancing:** Spring Cloud LoadBalancer
* **Containerization:** Docker

## 🏗 Architecture Context
The Gateway is the first point of contact for any request:
1.  **Entry:** A client sends a request (e.g., to fetch tasks).
2.  **Auth Check:** The Gateway intercepts the request and validates the **JWT token** (originally issued by the User Service).
3.  **Routing:** Once authorized, the Gateway consults the **Discovery Service** to find an available instance of the *Task Service* or *AI Agent Service*.
4.  **Proxy:** The request is proxied to the target service, and the response is returned to the client.

## ⚙️ Core Configuration (Routing Example)
The gateway is configured to route traffic based on path predicates:
* `/api/auth/**` -> **User Service**
* `/api/tasks/**` -> **Task Service**
* `/api/ai/**` -> **AI Agent Service**

## 📦 Getting Started
To run the API Gateway Service locally:

```bash
# Build the image
docker build -t taskmind-gateway-service .

# Run the container
docker run -p 8080:8080 taskmind-gateway-service
