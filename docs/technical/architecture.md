# Architecture

## System Architecture Overview

RetailPulse is a standard web application with three layers:

1.  **Frontend**: A React application that users see in their web browser.
2.  **Backend**: A FastAPI application that contains all the business logic.
3.  **Database**: A PostgreSQL database that stores all the data.

---

## High-Level Architecture Diagram

This diagram shows how the parts of the system connect.

```mermaid
graph TB
    subgraph Client["Client Layer"]
        Browser[Web Browser]
    end
    
    subgraph CDN["Static Hosting"]
        React[React App]
    end
    
    subgraph Application["Application Layer"]
        LB[Load Balancer]
        API1[Backend App 1]
        API2[Backend App 2]
    end
    
    subgraph Data["Data Layer"]
        DB[(PostgreSQL Database)]
        S3[S3 File Storage]
    end
    
    Browser --> React
    React -->|HTTPS| LB
    LB --> API1
    LB --> API2
    API1 --> DB
    API2 --> DB
    API1 --> S3
    API2 --> S3
```

---

## AWS-Specific Architecture Diagram

This diagram shows a simplified view of the system hosted on AWS, with the frontend on Vercel.

```mermaid
graph TB
    subgraph Internet["Internet"]
        Users[Users]
    end
    
    Frontend[(Vercel Frontend)]
    
    subgraph AWS["AWS Cloud"]
        subgraph VPC["VPC"]
            subgraph PrivateSubnet["Private Subnet"]
                Backend[Containerized Backend]
                DB[(PostgreSQL Database)]
            end
        end
    end
    
    Users -->|HTTPS| Frontend
    Frontend -->|HTTPS| Backend
    Backend --> DB
```
