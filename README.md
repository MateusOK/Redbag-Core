![banner](https://github.com/user-attachments/assets/3b92c249-3956-49c8-88fb-811fa400f6e4)
<h1 align="center">🐕 RedBag Core</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Java-000?style=for-the-badge&logo=java" />
  <img src="https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white" />
  <img src="https://img.shields.io/badge/Spring_Security-6DB33F?style=for-the-badge&logo=Spring-Security&logoColor=white" />
  <img src="https://img.shields.io/badge/Cloudinary-3448C5?style=for-the-badge&logo=Cloudinary&logoColor=white" />
  <img src="https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=JSON%20web%20tokens&logoColor=white" />
  <img src="https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=Swagger&logoColor=white" />
</p>

<p align="center">
  <a href="https://bitbucket.org/lbesson/ansi-colors"><img src="https://img.shields.io/badge/Maintained%3F-no-red.svg" /></a>
  <a href="https://github.com/Naereen/StrapDown.js/blob/master/LICENSE"><img src="https://img.shields.io/github/license/Naereen/StrapDown.js.svg" /></a>
  <a href="https://GitHub.com/Naereen/ama"><img src="https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg" /></a>
</p>

## 📌 **Project Overview**

The **RedBag-Core** is the main backend of the RedBag project, developed at Fatec Registro. It is responsible for managing user interactions, authentication, database operations, and API integrations, ensuring seamless communication between the system components.

Built with **Java Spring Boot**, this API provides a robust and scalable solution, acting as the central hub that connects the frontend application with the RedBag-Predictor, a microservice responsible for cataract diagnosis in dogs.

👉 Access the [RedBag-Predictor](https://github.com/MateusOK/RedBag-Predictor) for more details.

---

## 🚀 **Getting Started**

### **Prerequisites**

Before running this project, ensure you have the following installed:

- [Java 17+](https://www.azul.com/downloads/?package=jdk#zulu)
- [Cloudinary Account](https://cloudinary.com/)
- [RedBag-Predictor](https://github.com/MateusOK/RedBag-Predictor) (must be running for prediction functionality)
- [Docker](https://www.docker.com/) (optional but recommended)

### **Cloning the Repository**

Clone this project to your local machine:

```bash
git clone https://github.com/MateusOK/Redbag-Core
```

### **Building the Project**

Navigate to the project directory and build the JAR file:

```bash
mvn clean package
```

### **Environment Variables**

Modify the `application.yaml` file to match your Cloudinary credentials, Predictor, and database URL:

```yaml
datasource:
  url: ${SPRING_DATASOURCE_URL}
  driverClassName: 
  username: ${SPRING_DATASOURCE_USERNAME}
  password: ${SPRING_DATASOURCE_PASSWORD}

cloudinary:
  cloud-name: ${CLOUDINARY_CLOUD_NAME}
  api-key: ${CLOUDINARY_API_KEY}
  api-secret: ${CLOUDINARY_API_SECRET}

app:
  prediction-url: ${REDBAG_PREDICTOR_URL}
```

### **Running the API with Docker**

#### **Building the Docker Image**

```bash
docker build -t redbag-core .
```

#### **Running the Container**

```bash
docker run -p 8080:8080 \
  -e SPRING_DATASOURCE_URL=your_database_url \
  -e SPRING_DATASOURCE_USERNAME=your_db_user \
  -e SPRING_DATASOURCE_PASSWORD=your_db_password \
  -e CLOUDINARY_CLOUD_NAME=your_cloudinary_name \
  -e CLOUDINARY_API_KEY=your_cloudinary_key \
  -e CLOUDINARY_API_SECRET=your_cloudinary_secret \
  -e PYTHON_API_URL=your_redbag_predictor_url \
  redbag-core
```

If you prefer using an `.env` file, create it and use the following command:

```bash
docker run --env-file .env -p 8080:8080 redbag-core
```

### **Verifying the API**

Once the container is running, check if the API is accessible by visiting:

```
http://localhost:8080/swagger-ui/index.html#/
```

---

## 📍 **API Endpoints**

To access Swagger documentation and view all available endpoints, start the application and visit:

[Swagger UI](http://localhost:8080/swagger-ui/index.html#/)

### 🔑 **Authentication & User Management**

| Method | Route                | Description                   |
|--------|----------------------|-------------------------------|
| POST   | `/api/auth/register` | Register a new user           |
| POST   | `/api/auth/login`    | Authenticate and generate a token |
| PUT    | `/api/users`         | Update user data              |

#### **Example Responses**

##### **POST /api/auth/register**

_Request:_
```json
{
  "name": "John Doe",
  "username": "JohnDoe",
  "email": "johndoe@example.com",
  "password": "password"
}
```
_Response:_
```json
{
  "id": 1,
  "name": "John Doe",
  "username": "JohnDoe",
  "email": "johndoe@example.com"
}
```

##### **POST /api/auth/login**

_Request:_
```json
{
  "usernameOrEmail": "JohnDoe",
  "password": "password"
}
```
_Response:_
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR..."
}
```

### 🐶 **Animal Management**

| Method | Route                          | Description                    |
|--------|--------------------------------|--------------------------------|
| POST   | `/api/animals/{userId}`       | Create a new animal            |
| GET    | `/api/animals/{userId}`       | Retrieve all animals for user  |
| PUT    | `/api/animals/{userId}/{animalId}` | Update animal data            |
| DELETE | `/api/animals/{userId}/{animalId}` | Delete an animal              |

#### **Example Responses**

##### **POST /api/animals/{userId}**

_Request:_
```json
{
  "name": "Dog",
  "color": "#FFFFFF"
}
```
_Response:_
```json
{
  "userId": 1,
  "name": "Dog",
  "color": "#FFFFFF"
}
```

### 🧠 **Integration with RedBag-Predictor**

| Method | Route                  | Description                                 |
|--------|------------------------|---------------------------------------------|
| POST   | `/predict`              | Returns a preliminary diagnosis result     |
| POST   | `/predict/{animalId}`   | Saves the result to the database           |

#### **Example Requests & Responses**

##### **POST /predict**

_Request:_
```bash
curl -X POST "localhost:8080/predict" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -F "image=@/path/to/image.jpg"
```

_Response:_
```json
{
  "prediction": "unhealthy",
  "confidence": 0.89
}
```

##### **POST /predict/{animalId}**

_Request:_
```bash
curl -X POST "localhost:8080/predict/{animalId}" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -F "image=@/path/to/image.jpg"
```

_Response:_
```json
{
  "prediction": "healthy",
  "confidence": 0.78
}
```

---
## 🤝 **Collaborators**

Special thanks to all contributors to this project:

- [Mateus Ribeiro](https://www.linkedin.com/in/dev-mateus-ribeiro)
- [Gustavo Eyros](https://www.linkedin.com/in/gustavoeyros)
- [Rian Yuri](https://www.linkedin.com/in/rian-yuri-b36563158/)
- [Luiz Lopes](https://www.linkedin.com/in/luizlopes12)

---
This project is licensed under the [MIT License](LICENSE).

