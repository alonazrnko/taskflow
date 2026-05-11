# TaskFlow — Java Task Manager

A task management application built in plain Java, featuring in-memory and file-backed storage, task history tracking,
and a built-in HTTP server for API access.

## Features

- **Three task types:** Tasks, Epics, and Subtasks with parent-child relationships
- **Dual storage strategies:** In-memory (fast) and file-backed (persistent via CSV)
- **History tracking:** Automatically records the last viewed tasks using a custom history manager
- **Built-in HTTP server:** Exposes task management functionality via a lightweight Java HTTP server
- **Prioritized task access:** Tasks can be retrieved in priority order
- **Custom exception handling:** `ManagerSaveException` for storage-related errors
- **Unit tested:** Core logic covered with JUnit 5

## Tech Stack

- **Java 11**
- **Core Java OOP** — interfaces, inheritance, generics
- **Java Collections** — HashMap, LinkedList, TreeSet
- **JUnit Jupiter 5.8.1** — unit and integration tests
- **Gson 2.13** — JSON serialization for HTTP responses
- **Java HTTP Server** — built-in `com.sun.net.httpserver`

## Project Structure

**Core entities:** `Task`, `Epic`, `Subtask`, `TaskStatus`  
**Manager interfaces:** `TaskManager`, `HistoryManager`, `Managers` (factory)  
**Implementations:** `InMemoryTaskManager`, `FileBackedTaskManager`, `InMemoryHistoryManager`  
**HTTP layer:** `HttpTaskServer`, `BaseHttpHandler`, `TasksHandler`, `EpicsHandler`, `SubtasksHandler`,
`HistoryHandler`, `PrioritizedHandler`  
**Exceptions:** `ManagerSaveException`  
**Data:** `test_tasks.csv` — sample file for FileBackedTaskManager

## Getting Started

### Requirements

- Java 11 or higher
- No build tool required — plain Java project

### Run the HTTP Server

```bash
# Compile
javac -cp src src/HttpTaskServer.java -d out

# Start the server (default port: 8080)
java -cp out HttpTaskServer
```

### Example API Requests

```bash
# Get all tasks
curl http://localhost:8080/tasks

# Get all epics
curl http://localhost:8080/epics

# Get task history
curl http://localhost:8080/history

# Get tasks sorted by priority
curl http://localhost:8080/prioritized
```

### Run Tests

Open the project in IntelliJ IDEA and run test classes:

- `InMemoryTaskManagerTest`
- `FileBackedTaskManagerTest`
- `InMemoryHistoryManagerTest`
- `HttpTaskServerTasksTest`

## Key Design Decisions

- `TaskManager` and `HistoryManager` are defined as interfaces, allowing easy swapping of implementations (in-memory vs
  file-backed)
- `Managers` factory class follows the Factory pattern for clean instantiation
- History is stored in a doubly-linked structure to ensure O(1) removal and no duplicate entries
- File-backed manager persists state to CSV on every modification, enabling crash recovery
