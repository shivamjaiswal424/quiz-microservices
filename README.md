# 📘 Quiz Microservices

A microservices-based quiz application built with **Spring Boot, Spring Cloud, and PostgreSQL**.  
The system allows creating quizzes, fetching random questions by category, and evaluating scores — all while demonstrating core microservices concepts like **service discovery, API Gateway, inter-service communication (Feign), and database per service**.

---

## 🚀 Architecture

### 🟦 Service Registry (Eureka)
- Central registry for all microservices.  
- Each service registers itself and can discover others dynamically.  

### 🟩 API Gateway (Spring Cloud Gateway)
- Single entry point for all clients.  
- Routes requests to the appropriate microservice using service discovery.  

### 🟨 Question Service
- Manages **question data (CRUD)**.  
- Uses **PostgreSQL (`questiondb`)**.  
- Provides endpoints to fetch all questions, filter by category, generate random questions, and evaluate answers.  

### 🟥 Quiz Service
- Manages **quizzes and submissions**.  
- Uses **PostgreSQL (`quizdb`)**.  
- Calls **Question Service** using Feign clients to fetch questions.  

---

## 🛠️ Tech Stack

- **Backend:** Spring Boot (3.x), Spring Data JPA, Spring Web  
- **Microservices:** Spring Cloud Netflix Eureka, Spring Cloud Gateway, OpenFeign  
- **Database:** PostgreSQL (separate DB for each service)  
- **Build Tool:** Maven  
- **Language:** Java 17+  

---

## ⚙️ Setup & Run

### 1️⃣ Start PostgreSQL
Create two databases:
```sql
CREATE DATABASE questiondb;
CREATE DATABASE quizdb;
```
Update username/password in application.properties for both services.

### 2️⃣ Start Service Registry
```bash
cd service-registry
mvn spring-boot:run
```
Runs on port 8761 → Eureka dashboard: http://localhost:8761

### 3️⃣ Start Question Service
```bash
cd question-service
mvn spring-boot:run
```

- Registers itself with Eureka

- Runs on a random free port (if server.port not set)

**APIs (via gateway or direct):**

- GET /question/allQuestions

- GET /question/category/{category}

- POST /question/add

- GET /question/generate?categoryName=Python&numQuestions=4

- POST /question/getQuestions

- POST /question/getScore

###  4️⃣ Start Quiz Service
```bash
cd quiz-service
mvn spring-boot:run
```

- Registers itself with Eureka

- Runs on port 8090

**APIs (via gateway or direct):**

- POST /quiz/create

- GET /quiz/get/{id}

- POST /quiz/submit/{id}

### 5️⃣ Start API Gateway
```bash
cd api-gateway
mvn spring-boot:run
```
Runs on port 8765.
All services accessible through the gateway:

- http://localhost:8765/question/...

- http://localhost:8765/quiz/...

## 📡 Example Flow

- Add questions → POST /question/add

- Create quiz → POST /quiz/create (specify category & number of questions)

- Fetch quiz → GET /quiz/get/{id}

- Submit quiz → POST /quiz/submit/{id}

## 🔑 Key Features

- ✅ Service Discovery → Eureka handles dynamic registration & lookup

- ✅ API Gateway → Single entry point, routing, load balancing

- ✅ Database-per-service → Each microservice has its own schema

- ✅ Inter-Service Communication → Quiz Service uses Feign to call Question Service

- ✅ Randomized Questions → SQL ORDER BY RANDOM() ensures variety
