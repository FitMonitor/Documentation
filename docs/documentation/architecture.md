# Architecture

## Microservices Architecture

![Microservices Architecture](../files/microservices.drawio.png)

This diagram illustrates the architecture of our microservices, external services, and an API gateway. Below is a detailed breakdown of its components:

## External Services

- **AWS Cognito**: Used for user authentication and identity management, ensuring secure access to the platform.
- **Stripe**: Integrated to manage gym membership payments seamlessly.

## User Interface

- The **frontend application** is where users interact with the system.
- It communicates with the backend through the **API Gateway**, which acts as the intermediary between the user interface and microservices.

## Microservices

1. **Gym Management**:
Manages functionalities related to gym operations, including membership management and gym attendance tracking.

2. **QR-Code Management**:
Handles QR code operations such as generating unique codes for users and scanning them for authentication or tracking purposes.

3. **Payments**:
Processes payment-related tasks and interacts with Stripe for secure transaction handling.

## Message Queue

- Microservices communicate asynchronously using a shared **Message Queue**.
- **Apache Kafka** is used to facilitate reliable communication, ensuring that data flows efficiently between microservices like Gym Management and QR-Code Management.

## Database
A centralized **MySQL Database** is used to store essential application data, such as:

  - User information
  - QR code details
  - Payment records
  - Other related resources

This database ensures data consistency and simplifies management across all microservices.

---

## AWS Cloud Deployment Architecture

![Microservices Architecture](../files/updated_arq.drawio.png)

This deployment architecture leverages AWS services to ensure scalability, security, and high availability for a gym management platform. Below is an explanation of the key components and their roles within the architecture:

### AWS Cloud and Region

- The deployment is hosted within an **AWS Region**, ensuring that all resources are localized to minimize latency and optimize performance. The region encompasses the entire architecture, including the **Virtual Private Cloud (VPC)**.

### Virtual Private Cloud (VPC)

- A dedicated **VPC** isolates the deployment within a secure network environment. This VPC includes both public and private subnets spread across **two availability zones**, enhancing fault tolerance and availability.

### Availability Zones

- The architecture spans **two availability zones**, each containing public and private subnets. This ensures redundancy and high availability by distributing resources across multiple zones.

### Public Subnets

Each availability zone includes a public subnet hosting:

- **NAT Gateways**: These allow instances in private subnets to securely access the internet for updates or external communications without exposing them to the public.

### Private Subnets

The private subnets host the core application services:

**ECS Containers**:

- Containers are deployed using **AWS Elastic Container Service (ECS)** for both the frontend (Angular) and backend (Spring Boot API). These containers handle various application modules:
  - Payments
  - QR Code Management
  - Gym Management

**RDS (MySQL)**:

- A relational database service is deployed in the private subnet to securely store critical application data.

### Load Balancers

- **Web Application Load Balancer (Web ALB)**: Distributes incoming traffic across ECS containers in private subnets, ensuring high availability and scalability.

- **Internal Application Load Balancer (Internal ALB)**: Facilitates secure communication between internal services within the private network.

### VPC Link

- A **VPC Link** connects the private subnets to an **API Gateway**, enabling secure and efficient communication between internal resources and external clients.

### API Gateway

The **API Gateway** serves as the main entry point for external clients and provides:

- **Authentication and Authorization** using **AWS Cognito**.
- **Role Management** to control user access to specific features and resources.
- **User Management** to handle user accounts, permissions, and session management.
