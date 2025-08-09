# OTMT Platform - General Backend (Read-Only API)

<div align="center">
  <h3>🔒 Secure Read-Only API Service</h3>
  <p><em>High-performance backend serving data to public-facing clients</em></p>
  
  [![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)]()
  [![Express](https://img.shields.io/badge/Express.js-404D59?style=for-the-badge)]()
  [![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)]()
  [![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)]()
</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Features](#-features)
- [API Endpoints](#-api-endpoints)
- [Installation](#-installation)
- [Environment Configuration](#-environment-configuration)
- [Security](#-security)
- [Performance](#-performance)
- [Monitoring](#-monitoring)
- [Deployment](#-deployment)

## 🌟 Overview

The OTMT General Backend is a **read-only API service** designed with security and performance as top priorities. It serves as the single source of truth for all public-facing clients including the main website, mobile application, and AI chatbot service.

### Key Design Principles
- **Security First**: Read-only database permissions minimize attack surface
- **High Performance**: Optimized for concurrent read operations
- **Scalability**: Designed to handle multiple client applications
- **Reliability**: PM2 process management ensures 99.9% uptime

### Architecture Role
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Public Web    │    │   Mobile App    │    │   AI Chatbot    │
│    Frontend     │    │    (Android)    │    │    Service      │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────▼─────────────┐
                    │    General Backend        │
                    │    (Read-Only API)        │
                    └─────────────┬─────────────┘
                                  │
                         ┌────────▼────────┐
                         │   MongoDB       │
                         │   Database      │
                         │ (Read Access)   │
                         └─────────────────┘
```

## 🏗 Architecture

### Two-Backend Security Model
This backend is part of a **dual-backend architecture** that separates read and write operations:

- **General Backend (This Repo)**: Read-only access, public-facing
- **Admin Backend**: Write-enabled, intranet-only access

### Benefits of This Approach
1. **Enhanced Security**: Public clients can never execute write operations
2. **Performance Optimization**: Read operations are optimized separately
3. **Scalability**: Read replicas can be added without affecting write performance
4. **Fault Isolation**: Issues in public-facing services don't affect admin operations

## ✨ Features

### 🔐 Security Features
- **Read-Only Database Credentials**: Prevents any data modification
- **CORS Configuration**: Controlled cross-origin resource sharing
- **Rate Limiting**: Protection against abuse and DDoS
- **Request Validation**: Input sanitization and validation
- **SSL/TLS Encryption**: HTTPS-only communication

### ⚡ Performance Features
- **Efficient Queries**: Optimized MongoDB queries with proper indexing
- **Caching Layer**: Redis integration for frequently accessed data
- **Connection Pooling**: Optimized database connection management
- **Gzip Compression**: Reduced payload sizes
- **PM2 Clustering**: Multi-process load balancing

### 📊 Data Serving
- **Technologies API**: Complete technology database access
- **Events API**: Dynamic events information
- **Search & Filter**: Advanced querying capabilities
- **Pagination**: Efficient large dataset handling
- **Real-time Data**: Live data synchronization

## 🔌 API Endpoints

### Technologies Endpoints

#### Get All Technologies
```http
GET /api/technologies
```

**Query Parameters:**
- `page` (optional): Page number for pagination
- `limit` (optional): Number of items per page
- `search` (optional): Search term for filtering
- `genre` (optional): Technology category filter
- `trl` (optional): Technology Readiness Level filter
- `spotlight` (optional): Featured technologies only

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "tech_001",
      "name": "Advanced AI Algorithm",
      "description": "Brief description",
      "overview": "Detailed overview",
      "genre": "Software",
      "trl": 7,
      "spotlight": true,
      "innovators": [...],
      "applications": [...],
      "advantages": [...],
      "images": [...],
      "createdAt": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 5,
    "totalItems": 45,
    "limit": 10
  }
}
```

#### Get Technology by ID
```http
GET /api/technologies/:id
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "tech_001",
    "name": "Advanced AI Algorithm",
    "description": "Brief description",
    "detailedDescription": "Full detailed description...",
    "technicalSpecifications": "Technical specs...",
    "innovators": [
      {
        "name": "Dr. John Doe",
        "email": "john.doe@iiitd.ac.in"
      }
    ],
    "patent": {
      "status": "Filed",
      "patentId": "IN202301001",
      "filingDate": "2023-01-15"
    },
    "relatedLinks": [...],
    "brochures": [...],
    "images": [...]
  }
}
```

### Events Endpoints

#### Get All Events
```http
GET /api/events
```

**Query Parameters:**
- `isActive` (optional): Filter by active status
- `upcoming` (optional): Filter upcoming events only

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "event_001",
      "title": "Innovation Showcase 2024",
      "month": "MAR",
      "day": "15",
      "location": "IIIT Delhi Campus",
      "time": "10:00 AM",
      "description": "Annual innovation showcase...",
      "registration": "https://events.iiitd.ac.in/register",
      "isActive": true
    }
  ]
}
```

### Utility Endpoints

#### Health Check
```http
GET /api/health
```

#### API Status
```http
GET /api/status
```

## 🚀 Installation

### Prerequisites
- Node.js (v16 or higher)
- MongoDB (v4.4 or higher)
- npm or yarn

### Quick Setup

```bash
# Clone the repository
git clone https://github.com/devan1shX/TMTO-Backend.git
cd TMTO-Backend

# Install dependencies
npm install --force

# Set up environment variables
cp .env.example .env
# Edit .env with your configuration

# Start development server
npm run dev
```

### Development vs Production

**Development:**
```bash
npm run dev  # Uses nodemon for auto-restart
```

**Production:**
```bash
npm start    # Direct Node.js execution
# OR
npm run pm2  # PM2 process management
```

## ⚙️ Environment Configuration

### Required Environment Variables

Create a `.env` file with the following configuration:

```env
# Server Configuration
PORT=5000
NODE_ENV=production

# Database Configuration (READ-ONLY CREDENTIALS)
MONGODB_URI=mongodb://readonly_user:secure_password@localhost:27017/otmt_db
MONGODB_OPTIONS=authSource=admin&readPreference=secondary

# Security Configuration
CORS_ORIGIN=https://otmt.iiitd.ac.in,https://app.otmt.iiitd.ac.in
RATE_LIMIT_MAX=100
RATE_LIMIT_WINDOW=900000

# Performance Configuration
CACHE_TTL=3600
ENABLE_COMPRESSION=true
CONNECTION_POOL_SIZE=10

# Monitoring
LOG_LEVEL=info
ENABLE_METRICS=true
```

### Database User Setup

**Critical**: This backend must use a MongoDB user with **read-only** permissions:

```javascript
// MongoDB Admin Command
db.createUser({
  user: "otmt_readonly",
  pwd: "secure_password",
  roles: [
    {
      role: "read",
      db: "otmt_database"
    }
  ]
});
```

## 🔒 Security

### Database Security
- **Read-Only User**: Database user has no write, update, or delete permissions
- **Connection Encryption**: MongoDB connection uses SSL/TLS
- **IP Whitelisting**: Database access restricted to application servers

### API Security
- **CORS Policy**: Strict origin validation
- **Rate Limiting**: Protection against abuse
- **Input Validation**: All inputs sanitized and validated
- **Error Handling**: No sensitive information leaked in error responses

### Network Security
- **HTTPS Only**: SSL certificate enforced
- **Firewall Rules**: Port access restricted
- **Reverse Proxy**: NGINX handles SSL termination

## ⚡ Performance

### Optimization Strategies

#### Database Optimization
```javascript
// Efficient indexes for common queries
db.technologies.createIndex({ "genre": 1, "trl": 1 });
db.technologies.createIndex({ "spotlight": 1, "createdAt": -1 });
db.technologies.createIndex({ "createdBy.userId": 1 });
```

#### Caching Strategy
- **Redis Integration**: Frequently accessed data cached
- **TTL Configuration**: Automatic cache invalidation
- **Query Result Caching**: Complex aggregation results cached

#### Connection Management
```javascript
// Optimized MongoDB connection
mongoose.connect(mongoURI, {
  maxPoolSize: 10,
  serverSelectionTimeoutMS: 5000,
  socketTimeoutMS: 45000,
  bufferMaxEntries: 0
});
```

## 📊 Monitoring

### PM2 Process Management
```bash
# Start with PM2
npm run pm2:start

# Monitor processes
pm2 monit

# View logs
pm2 logs

# Restart gracefully
pm2 reload ecosystem.config.js
```

### Health Monitoring
```javascript
// Health check endpoint implementation
app.get('/api/health', (req, res) => {
  res.json({
    status: 'healthy',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    database: mongoose.connection.readyState === 1 ? 'connected' : 'disconnected'
  });
});
```

### Performance Metrics
- **Response Times**: Average API response monitoring
- **Database Performance**: Query execution time tracking
- **Error Rates**: 4xx and 5xx response monitoring
- **Resource Usage**: CPU and memory utilization

## 🚀 Deployment

### Production Deployment with PM2

1. **Install PM2 globally:**
```bash
npm install -g pm2
```

2. **Configure ecosystem file:**
```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'otmt-general-backend',
    script: 'server.js',
    instances: 'max',
    exec_mode: 'cluster',
    env: {
      NODE_ENV: 'development'
    },
    env_production: {
      NODE_ENV: 'production',
      PORT: 5000
    }
  }]
};
```

3. **Deploy:**
```bash
pm2 start ecosystem.config.js --env production
pm2 save
pm2 startup
```

### NGINX Configuration
```nginx
upstream otmt_backend {
    server 127.0.0.1:5000;
    keepalive 32;
}

server {
    listen 443 ssl http2;
    server_name api.otmt.iiitd.ac.in;
    
    location / {
        proxy_pass http://otmt_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## 🔗 Related Services

This backend works in conjunction with:

- **[Main Website Frontend](https://github.com/devan1shX/TMTO)** - Primary data consumer
- **[Admin Backend](https://github.com/devan1shX/Admin-Backend)** - Write operations counterpart
- **[Mobile Application](https://github.com/devan1shX/OTMT-App)** - Native Android client
- **[AI Chatbot](https://github.com/Beingstupid4me/Tech-Transfer-Pal)** - Data caching consumer

## 📞 Support

For technical issues or questions regarding this backend service:

**Development Team:**
- **Amartya Singh** - amartya22062@iiitd.ac.in
- **Anish** - anish22075@iiitd.ac.in

---

<div align="center">
  <p>🔒 Securing OTMT's data layer with read-only excellence</p>
</div>
