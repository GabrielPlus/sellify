# 🛒 Sellify - E-commerce Microservices Platform

<a alt="Nx logo" href="https://nx.dev" target="_blank" rel="noreferrer"><img src="https://raw.githubusercontent.com/nrwl/nx/master/images/nx-logo.png" width="45"></a>

A modern, scalable e-commerce platform built with microservices architecture using Nx monorepo. This project implements a distributed system with API Gateway, Authentication Service, and shared libraries for optimal performance and maintainability.

## 🏗️ Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   API Gateway   │────│  Auth Service   │────│   MongoDB       │
│   (Port: 8080)  │    │  (Port: 6001)   │    │   (Primary DB)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Rate Limiting │    │   Email Service │    │   Redis Cache   │
│   CORS Support  │    │   JWT Auth      │    │   (Secondary)   │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   WebSocket     │    │   Kafka         │    │   AWS Cloud     │
│   Real-time     │    │   Message Broker│    │   Deployment    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   ML/AI         │    │   Push Notif    │    │   CI/CD         │
│   TensorFlow    │    │   Firebase      │    │   Pipeline      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🚀 Technology Stack

### **Architecture & Framework**
- **Microservice Architecture** - Distributed system design for scalability
- **Express.js** - Backend web framework for REST APIs
- **Node.js** with **TypeScript** - Runtime and type safety
- **Nx Monorepo** - Build system and workspace management
- **WebSocket** - Real-time bidirectional communication

### **Database & Storage**
- **MongoDB** - Primary NoSQL database
- **Redis** (ioredis) - Secondary database for caching and session management
- **Prisma** - Database ORM for type-safe database access

### **Authentication & Security**
- **JWT** - Authentication and authorization tokens
- **Nodemailer** - Email service integration
- **EJS** - Email template engine
- **Express Rate Limit** - API rate limiting
- **CORS** - Cross-origin resource sharing

### **Infrastructure & DevOps**
- **Docker** - Containerization platform
- **AWS** - Cloud provider for deployment and production
- **CI & CD** - Continuous Integration and Continuous Deployment
- **Kafka** - Message broker for event streaming
- **Web Push Notification** - Real-time notifications (Firebase)

### **API & Documentation**
- **Swagger/OpenAPI** - API documentation and testing
- **API Testing** - Comprehensive API testing tools
- **Domain Management** - DNS and domain configuration

### **Development & Build Tools**
- **Webpack** - Module bundling
- **ESBuild** - Fast JavaScript bundler
- **SWC** - Fast TypeScript/JavaScript compiler
- **ts-node** - TypeScript execution environment
- **Jest** - Testing framework
- **Morgan** - HTTP request logging

### **Machine Learning & Analytics**
- **TensorFlow** - Machine Learning framework for AI features
- **Data Analytics** - Business intelligence and reporting

### **Frontend Technologies**
- **Frontend Framework** - Modern UI framework (to be implemented)
- **Responsive Design** - Mobile-first approach

## 📁 Project Structure

```
sellify/
├── apps/
│   ├── api-gateway/          # Main API gateway service
│   ├── api-gateway-e2e/      # E2E tests for API gateway
│   ├── auth-service/         # Authentication microservice
│   └── auth-service-e2e/     # E2E tests for auth service
├── packages/
│   ├── libs/
│   │   ├── prisma/          # Database client
│   │   └── redis/           # Redis client
│   ├── error-handler/       # Global error handling
│   └── middleware/          # Shared middleware
├── prisma/
│   └── schema.prisma        # Database schema
└── generated/
    └── prisma/              # Generated Prisma client
```

## 🛠️ Services

### **API Gateway** (`apps/api-gateway`)
- **Port**: 8080
- **Purpose**: Central entry point for all client requests
- **Features**:
  - Request routing and load balancing
  - Rate limiting (100 requests/15min for guests, 1000 for authenticated users)
  - CORS configuration
  - Request logging with Morgan
  - Health check endpoint

### **Auth Service** (`apps/auth-service`)
- **Port**: 6001
- **Purpose**: User authentication and authorization
- **Features**:
  - User registration and login
  - JWT token management
  - Email verification system
  - Password reset functionality
  - Swagger API documentation
  - Cookie-based session management

## 🗄️ Database Schema

### **Users Model**
```prisma
model users {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  name      String 
  email     String   @unique
  password  String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  avatar    images?
  following String[]
}
```

### **Images Model**
```prisma
model images {
  id        String   @id @default(auto()) @map("_id") @db.ObjectId
  file_id   String
  url       String
  userId    String    @db.ObjectId @unique
  users     users    @relation(fields: [userId], references: [id])
}
```

## 🚀 Getting Started

### Prerequisites
- Node.js (v18 or higher)
- npm or yarn
- MongoDB database
- Redis server
- Docker (optional)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd sellify
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Environment Setup**
   Create a `.env` file in the root directory:
   ```env
   DATABASE_URL="mongodb://localhost:27017/sellify"
   REDIS_DATABASE_URI="redis://localhost:6379"
   JWT_SECRET="your-jwt-secret"
   EMAIL_HOST="smtp.gmail.com"
   EMAIL_PORT=587
   EMAIL_USER="your-email@gmail.com"
   EMAIL_PASS="your-app-password"
   ```

4. **Database Setup**
   ```bash
   npx prisma generate
   npx prisma db push
   ```

### Development

**Start all services in development mode:**
```bash
npm run dev
```

**Start individual services:**
```bash
# API Gateway
npx nx serve api-gateway

# Auth Service
npx nx serve auth-service
```

**Build for production:**
```bash
npx nx build api-gateway
npx nx build auth-service
```

### Testing

**Run all tests:**
```bash
npx nx test
```

**Run E2E tests:**
```bash
npx nx e2e auth-service-e2e
npx nx e2e api-gateway-e2e
```

### Docker

**Build Docker images:**
```bash
npx nx docker:build auth-service
```

**Run with Docker:**
```bash
npx nx docker:run auth-service
```

## 📚 API Documentation

Once the services are running, you can access:

- **API Gateway**: http://localhost:8080
- **Auth Service**: http://localhost:6001
- **Swagger Docs**: http://localhost:6001/api-docs
- **API Health Check**: http://localhost:8080/gateway-health

## 🔧 Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start all services in development mode |
| `npx nx serve <service>` | Start a specific service |
| `npx nx build <service>` | Build a service for production |
| `npx nx test <service>` | Run tests for a service |
| `npx nx e2e <service>-e2e` | Run E2E tests |
| `npx nx graph` | Visualize project dependencies |

## 🏗️ Adding New Services

**Generate a new microservice:**
```bash
npx nx g @nx/node:app <service-name>
```

**Generate a new shared library:**
```bash
npx nx g @nx/node:lib <library-name>
```

## 🔐 Security Features

- JWT-based authentication
- Rate limiting to prevent abuse
- CORS configuration
- Input validation and sanitization
- Secure cookie handling
- Password hashing (bcrypt)

## 📈 Performance Features

- Redis caching for improved response times
- Request compression
- Connection pooling
- Optimized database queries with Prisma
- Microservices architecture for scalability
- WebSocket support for real-time features
- Kafka message streaming for event-driven architecture

## 🚀 Future Roadmap

### **Planned Integrations**
- **Frontend Framework** - Modern React/Vue.js application
- **Machine Learning** - TensorFlow integration for recommendation engine
- **Real-time Features** - WebSocket implementation for live updates
- **Message Streaming** - Kafka integration for event-driven architecture
- **Push Notifications** - Firebase integration for mobile/web notifications
- **Cloud Deployment** - AWS infrastructure setup
- **CI/CD Pipeline** - Automated testing and deployment
- **Domain Management** - Custom domain configuration
- **Advanced Analytics** - Business intelligence dashboard

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Useful Links

- [Nx Documentation](https://nx.dev)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Express.js Documentation](https://expressjs.com)
- [MongoDB Documentation](https://docs.mongodb.com)
- [Redis Documentation](https://redis.io/documentation)

---

**Built with ❤️ using Nx, Node.js, and TypeScript**
