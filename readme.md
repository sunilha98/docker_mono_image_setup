# Resource Management Application (RMA) - Docker Microservices Setup

A comprehensive Docker-based microservices architecture for resource management, built with Spring Boot and React.

## 🏗️ Architecture Overview

This project implements a microservices architecture with the following components:

### Core Services
- **Eureka Server** (8761) - Service discovery and registration
- **API Gateway** (8080) - Central entry point for all API requests
- **User Management Service** (8081) - User authentication and authorization
- **Master Resource Service** (8085) - Core resource management and categorization
- **Project SoW Service** (8082) - Statement of Work management
- **Resource Allocation Service** (8083) - Resource assignment and scheduling
- **Reporting Integration Service** (8084) - Analytics and reporting

### Frontend
- **React Frontend** (3000) - Modern web interface

### Database
- **PostgreSQL** (5432) - Primary database with multiple schemas

## 🚀 Quick Start

### Prerequisites
- Docker and Docker Compose
- Java 17+ (for local development)
- Maven (for local development)

### Running the Application

1. **Clone and navigate to the project directory**
   ```bash
   git clone <repository-url>
   cd docker_mono_image_setup
   ```

2. **Start all services with Docker Compose**
   ```bash
   docker-compose up -d
   ```

3. **Access the application**
   - Frontend: http://localhost:3000
   - Eureka Dashboard: http://localhost:8761
   - API Gateway: http://localhost:8080

### Individual Service Access
- User Management Service: http://localhost:8081
- Master Resource Service: http://localhost:8085  
- Project SoW Service: http://localhost:8082
- Resource Allocation Service: http://localhost:8083
- Reporting Integration Service: http://localhost:8084

## 📁 Project Structure

```
docker_mono_image_setup/
├── docker-compose.yml          # Multi-container orchestration
├── Dockerfile                  # Multi-stage build for all services
├── init-db.sql                # Database schema initialization
├── .gitignore                 # Git ignore rules
│
├── api-gateway/               # API Gateway service
├── eureka-server/             # Service discovery server
├── master-resource-service/   # Core resource management
├── project-sow-service/       # Project management
├── resource-allocation-service/ # Resource allocation
├── reporting-integration-service/ # Reporting and analytics
├── user-mgmt-service/         # User authentication
└── rma_react_frontend/        # React frontend application
```

## 🛠️ Development

### Building Services Locally

Each service can be built individually using Maven:

```bash
# Build a specific service
cd master-resource-service
mvn clean package

# Or build all services from root
mvn -f eureka-server/pom.xml clean package
mvn -f api-gateway/pom.xml clean package
# ... repeat for other services
```

### Database Configuration

The application uses PostgreSQL with multiple schemas:
- `usr_mgmt_db` - User management data
- `master_service_db` - Resource master data
- `project_sow_db` - Project data
- `resource_allocation_db` - Allocation data

Database is automatically initialized with the schemas on container startup.

### Environment Variables

Key environment variables (set in docker-compose.yml):
- `POSTGRES_USER`: postgres
- `POSTGRES_PASSWORD`: postgres
- Service ports as specified above

## 🔧 Docker Commands

### Common Operations
```bash
# Start all services in detached mode
docker-compose up -d

# Stop all services
docker-compose down

# View logs for a specific service
docker-compose logs eureka-server

# Restart a specific service
docker-compose restart master-resource-service

# Build images without cache
docker-compose build --no-cache
```

### Service Management
```bash
# Scale specific services
docker-compose up -d --scale user-mgmt-service=2

# Check service status
docker-compose ps

# View resource usage
docker stats
```

## 📊 Monitoring and Debugging

### Eureka Service Registry
Access the Eureka dashboard at http://localhost:8761 to see all registered services and their status.

### Database Access
```bash
# Connect to PostgreSQL container
docker exec -it rma-postgres psql -U postgres -d postgres

# List databases
\l

# Connect to specific schema
\c postgres
\dn

# Switch to a schema (example user management schema): 
SET search_path TO usr_mgmt_db;

# List tables: 
\dt

# View data from a table (example users table): 
SELECT * FROM users;
```

### Sample Data Insertion
To insert sample user data into the users table:

```sql
INSERT INTO users (username, email, password, first_name, last_name, role, is_active, created_by, updated_by)
VALUES 
    ('admin', 'admin@resourcemanagement.com', '$2a$10$N9qo8uLOickgx2ZMRZoMy.MrYrWswsE7J0kR8uW7W1dT6Kj7X1/2O', 'System', 'Administrator', 'SUPER_ADMIN', true, 'system', 'system'),
    ('pm_user', 'pm@resourcemanagement.com', '$2a$10$N9qo8uLOickgx2ZMRZoMy.MrYrWswsE7J0kR8uW7W1dT6Kj7X1/2O', 'Project', 'Manager', 'PROJECT_MANAGER', true, 'system', 'system'),
    ('resource_user', 'resource@resourcemanagement.com', '$2a$10$N9qo8uLOickgx2ZMRZoMy.MrYrWswsE7J0kR8uW7W1dT6Kj7X1/2O', 'John', 'Doe', 'RMT', true, 'system', 'system'),
    ('finance_user', 'finance@resourcemanagement.com', '$2a$10$N9qo8uLOickgx2ZMRZoMy.MrYrWswsE7J0kR8uW7W1dT6Kj7X1/2O', 'Jane', 'Smith', 'FINANCE_CONTROLLER', true, 'system', 'system')
ON CONFLICT (username) DO NOTHING;
```

### Logs and Debugging
```bash
# Follow logs for all services
docker-compose logs -f

# View logs for specific service
docker-compose logs master-resource-service

# Access container shell
docker exec -it docker_mono_image_setup-master-resource-service-1 sh
```

## 🧪 Testing

### Running Tests
```bash
# Run tests for a specific service
cd master-resource-service
mvn test

# Run with coverage
mvn test jacoco:report
```

### Health Checks
All services include health endpoints:
- http://localhost:8080/actuator/health (API Gateway)
- http://localhost:8081/actuator/health (User Management)
- ... etc.


## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

### Code Style
- Follow Java coding conventions
- Use meaningful commit messages
- Include documentation for new features

## 🆘 Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 5432, 8761, 8080-8085, 3000 are available
2. **Database connection issues**: Check if PostgreSQL container is running
3. **Service registration problems**: Verify Eureka server is accessible
4. **Build failures**: Ensure Maven and Java 17 are properly installed
5. **Frontend issues**: Ensure Node.js and npm are installed for local frontend development

### Debug Mode
Enable debug logging by updating application.yml in respective services:
```yaml
logging:
  level:
    com.resourcemgmt: DEBUG
    org.springframework: DEBUG
```

**Note**: This is a development setup. For production deployment, additional security, monitoring, and configuration considerations are required.
