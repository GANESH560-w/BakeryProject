# Bakery Management Project - UML Diagrams

This document contains all the UML diagrams for the Bakery Management Project, including architecture, data models, and behavioral workflows.

---

## 1. Class Diagram
```mermaid
classDiagram
    class BackendServer {
        +main(args: String[])
        +tryRegisterMySqlDriver()
        +addCors(exchange: HttpExchange)
        +setJsonHeaders(headers: Headers)
        +readBody(is: InputStream) String
    }

    class DB {
        +connect() Connection
        +productsJson() String
        +teamJson() String
        +testimonialsJson() String
        +saveContact(body: String) String
        +esc(s: String) String
    }

    class HttpHandler {
        <<interface>>
        +handle(exchange: HttpExchange)
    }

    class JsonHandler {
        -supplier: Supplier
        +handle(exchange: HttpExchange)
    }

    class ContactHandler {
        +handle(exchange: HttpExchange)
    }

    class Supplier {
        <<interface>>
        +get() String
    }

    BackendServer ..> JsonHandler : creates
    BackendServer ..> ContactHandler : creates
    JsonHandler ..> Supplier : uses
    JsonHandler ..> DB : calls
    ContactHandler ..> DB : calls
    HttpHandler <|.. JsonHandler
    HttpHandler <|.. ContactHandler
```

---

## 2. ER Diagram
```mermaid
erDiagram
    PRODUCTS {
        int id PK
        string name
        double price
        string image
    }
    TEAM {
        int id PK
        string name
        string role
        string image
    }
    TESTIMONIALS {
        int id PK
        string name
        string text
    }
    CONTACT_MESSAGES {
        int id PK
        string payload
    }
```

---

## 3. Use Case Diagram
```mermaid
graph TD
    subgraph "Bakery Management System"
        UC1(Browse Products)
        UC2(View Team Members)
        UC3(Read Testimonials)
        UC4(Send Contact Message)
        UC5(Manage Database)
    end

    Customer --> UC1
    Customer --> UC2
    Customer --> UC3
    Customer --> UC4
    Admin --> UC5
```

---

## 4. Sequence Diagram
```mermaid
sequenceDiagram
    participant User
    participant Browser as Frontend (HTML/JS)
    participant Server as BackendServer
    participant DB as DB Class
    participant MySQL as MySQL Database

    User->>Browser: Fills & Submits Contact Form
    Browser->>Server: HTTP POST /api/contact (JSON)
    Server->>DB: saveContact(body)
    DB->>MySQL: INSERT INTO contact_messages
    MySQL-->>DB: Success
    DB-->>Server: {"status":"ok"}
    Server-->>Browser: 200 OK (JSON)
    Browser->>User: Show "Message saved!" Alert
```

---

## 5. Collaboration Diagram
```mermaid
graph LR
    User -- submits form --> Frontend
    Frontend -- AJAX/JSON --> BackendServer
    BackendServer -- invokes --> DB
    DB -- JDBC Query --> MySQL
    MySQL -- result --> DB
    DB -- JSON response --> BackendServer
    BackendServer -- HTTP Response --> Frontend
```

---

## 6. Activity Diagram
```mermaid
flowchart TD
    A[Start] --> B[User visits Products Page]
    B --> C{API Request /api/products}
    C -->|Success| D[Parse JSON Response]
    C -->|Failure| E[Load Fallback Static Data]
    D --> F[Render Product Cards]
    E --> F
    F --> G[End]
```

---

## 7. State Chart Diagram
```mermaid
stateDiagram-v2
    [*] --> Initializing
    Initializing --> DriverRegistration: Register MySQL Driver
    DriverRegistration --> ServerCreation: Create HttpServer
    ServerCreation --> AddingContexts: Map /api Endpoints
    AddingContexts --> Running: server.start()
    Running --> Error: Database Unavailable
    Error --> Running: Retry Connection
    Running --> [*]: Stop Signal
```

---

## 8. Component Diagram
```mermaid
graph TB
    subgraph "Frontend"
        F1[HTML/CSS/JS Assets]
        F2[Bootstrap Framework]
    end

    subgraph "Backend-Java"
        B1[BackendServer.java]
        B2[DB.java]
        B3[MySQL Connector]
    end

    subgraph "Persistence"
        D1[(MySQL Server)]
    end

    F1 --> B1
    B1 --> B2
    B2 --> D1
```

---

## 9. Deployment Diagram
```mermaid
graph TD
    subgraph "User Computer"
        U[Web Browser]
    end

    subgraph "Web Server (Docker Container)"
        W1[Java Runtime]
        W2[BackendServer.jar]
    end

    subgraph "Database Server (Docker Container)"
        D1[MySQL 8.0]
    end

    U -- "HTTP/HTTPS (Port 8080)" --> W2
    W2 -- "TCP/IP (Port 3306)" --> D1
```

---

## 10. Package Diagram
```mermaid
graph TB
    subgraph "Root"
        R1[HTML Files]
        R2[Docker Config]
    end

    subgraph "Assets"
        A1[css]
        A2[js]
        A3[img]
        A4[lib]
    end

    subgraph "Backend"
        subgraph "src"
            S1[BackendServer]
            S2[DB]
        end
        subgraph "lib"
            L1[mysql-connector]
        end
    end

    R1 -.-> A1
    R1 -.-> S1
