# Task Manager API (Task 1)

This project is a Java Spring Boot application that provides a REST API for creating, searching, deleting, and running "task" objects. Task objects represent shell commands that can be executed, and their data is stored in a MongoDB database.

## File Structure

```
.
├── .gitignore
├── pom.xml
├── README.md
└── src
    └── main
        ├── java
        │   └── com
        │       └── kaiburr
        │           └── taskmanager
        │               ├── TaskManagerApplication.java
        │               ├── controller
        │               │   └── TaskController.java
        │               ├── model
        │               │   ├── Task.java
        │               │   └── TaskExecution.java
        │               ├── repository
        │               │   └── TaskRepository.java
        │               └── service
        │                   └── TaskService.java
        └── resources
            └── application.properties
```

## Prerequisites

- Java 17
- Maven
- Docker

## How to Run

1.  **Start MongoDB using Docker**: If you don't have a local MongoDB instance, you can run one using Docker with the following command:
    ```bash
    docker run -d -p 27017:27017 --name mongodb mongo
    ```
    This will start a MongoDB container and make it available on `mongodb://localhost:27017`.

2.  **Run the application**: Open your terminal in the project root and run the following command:
    ```bash
    mvn spring-boot:run
    ```
The application will start on `http://localhost:8081`.

## API Endpoints and Usage

Here are the available endpoints and examples of how to use them with `curl`.

### Create or Update a Task
- **Endpoint**: `PUT /tasks`
- **Description**: Creates a new task or updates an existing one. The task object is sent in the request body.
- **Example**:
  ```bash
  curl -X PUT http://localhost:8081/tasks -H "Content-Type: application/json" -d '{"id": "101", "name": "My Test Task", "owner": "Balaji", "command": "echo Hello World"}'
  ```

### Get Tasks
- **Endpoint**: `GET /tasks`
- **Description**: Returns a list of all tasks. If an optional `id` request parameter is provided, it returns a single task matching the ID.
- **Example (Get all tasks)**:
  ```bash
  curl http://localhost:8081/tasks
  ```
- **Example (Get a task by ID)**:
  ```bash
  curl http://localhost:8081/tasks?id=101
  ```

### Find Tasks by Name
- **Endpoint**: `GET /tasks/name/{name}`
- **Description**: Finds all tasks where the name contains the given string. Returns 404 if nothing is found.
- **Example**:
  ```bash
  curl http://localhost:8081/tasks/name/Test
  ```

### Execute a Task
- **Endpoint**: `PUT /tasks/execute/{id}`
- **Description**: Executes the shell command of the specified task and records the execution details.
- **Example**:
  ```bash
  curl -X PUT http://localhost:8081/tasks/execute/101
  ```
  After executing, you can call `GET /tasks/101` again to see the `taskExecutions` list updated with the result.

### Delete a Task
- **Endpoint**: `DELETE /tasks/{id}`
- **Description**: Deletes a task by its ID.
- **Example**:
  ```bash
  curl -X DELETE http://localhost:8081/tasks/101
  ```



