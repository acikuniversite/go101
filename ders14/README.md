# Microservices and Folder Structure (Microservis ve Klasör Yapısı)

## Introduction

In this lesson, we will explore the concept of microservices and how to structure your project folders effectively. Microservices architecture allows you to break down your application into smaller, manageable services that can be developed, deployed, and scaled independently.

## What are Microservices?

Microservices are a software development technique—a variant of the service-oriented architecture (SOA) structural style—that arranges an application as a collection of loosely coupled services. In a microservices architecture, services are fine-grained and the protocols are lightweight.

### Key Characteristics of Microservices

- **Independently Deployable**: Each service can be developed, deployed, and scaled independently.
- **Decentralized Data Management**: Each service manages its own database.
- **Polyglot Programming**: Different services can be written in different programming languages.
- **Fault Isolation**: Failure in one service does not affect others.

## Folder Structure

A well-organized folder structure is crucial for maintaining a scalable and manageable codebase. Below is an example of a typical folder structure for a microservices project:

```
project-root/
├── service1/
│   ├── src/
│   │   ├── main.py
│   │   ├── utils.py
│   ├── tests/
│   │   ├── test_main.py
│   ├── Dockerfile
│   ├── requirements.txt
├── service2/
│   ├── src/
│   │   ├── main.py
│   │   ├── utils.py
│   ├── tests/
│   │   ├── test_main.py
│   ├── Dockerfile
│   ├── requirements.txt
├── gateway/
│   ├── src/
│   │   ├── main.py
│   │   ├── utils.py
│   ├── tests/
│   │   ├── test_main.py
│   ├── Dockerfile
│   ├── requirements.txt
├── docker-compose.yml
└── README.md
```

### Explanation of the Folder Structure

- **service1, service2, ...**: Each folder represents a microservice.
  - **src/**: Contains the source code for the service.
    - **main.py**: The entry point of the service.
    - **utils.py**: Utility functions used by the service.
  - **tests/**: Contains the test cases for the service.
    - **test_main.py**: Test cases for the main.py file.
  - **Dockerfile**: Instructions to build a Docker image for the service.
  - **requirements.txt**: List of dependencies for the service.

- **gateway/**: Acts as an API gateway to route requests to the appropriate microservice.
  - **src/**: Contains the source code for the gateway.
    - **main.py**: The entry point of the gateway.
    - **utils.py**: Utility functions used by the gateway.
  - **tests/**: Contains the test cases for the gateway.
    - **test_main.py**: Test cases for the main.py file.
  - **Dockerfile**: Instructions to build a Docker image for the gateway.
  - **requirements.txt**: List of dependencies for the gateway.

- **docker-compose.yml**: A Docker Compose file to run all the services together.
- **README.md**: Documentation for the project.

## Example Code

Here is a simple example of a microservice written in Python using Flask:

### main.py

```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/health', methods=['GET'])
def health_check():
    return jsonify({"status": "UP"}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Dockerfile

```dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY src/requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY src/ /app

CMD ["python", "main.py"]
```

### requirements.txt

```
Flask==2.0.1
```

## Conclusion

Microservices architecture offers numerous benefits, including independent deployment, fault isolation, and the ability to use different technologies for different services. A well-structured folder organization is essential for maintaining the scalability and manageability of your microservices project.

