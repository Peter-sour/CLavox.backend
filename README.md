# Clavox Backend API 🔥⚡

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node-dot-js&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![Socket.IO](https://img.shields.io/badge/Socket.IO-010101?style=for-the-badge&logo=socketdotio&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=JSON%20web%20tokens&logoColor=white)

Backend API untuk aplikasi chat real-time **Clavox** yang dibangun dengan **Node.js + Express + Socket.IO**. Menyediakan REST API, real-time messaging, dan sistem autentikasi OTP.

> **Note**: Ini adalah repository backend. Frontend tersedia di [Clavox Frontend](link-ke-frontend-repo)

---

## ✨ Fitur Backend

- 🔐 **Phone Authentication** dengan OTP verification
- 💬 **Real-time Messaging** menggunakan Socket.IO  
- 🔑 **JWT Authentication** untuk secure API
- 📧 **OTP Service** integration (SMS/WhatsApp)
- 💾 **MongoDB Database** untuk data persistence
- 🛡️ **Input Validation** dengan express-validator
- 📝 **Request Logging** dengan Morgan
- 🔄 **CORS Configuration** untuk frontend integration
- ⚡ **Rate Limiting** untuk API protection
- 🐛 **Error Handling** middleware

---

## 📂 Struktur Backend

```
clavox-backend/
├── src/
│   ├── controllers/         # Request handlers
│   │   ├── authController.js
│   │   ├── chatController.js
│   │   └── userController.js
│   ├── middleware/          # Custom middleware
│   │   ├── auth.js
│   │   ├── validation.js
│   │   └── errorHandler.js
│   ├── models/              # Database models
│   │   ├── User.js
│   │   ├── Chat.js
│   │   └── Message.js
│   ├── routes/              # API routes
│   │   ├── auth.js
│   │   ├── chat.js
│   │   └── users.js
│   ├── services/            # Business logic
│   │   ├── otpService.js
│   │   ├── smsService.js
│   │   └── socketService.js
│   ├── utils/               # Helper functions
│   │   ├── logger.js
│   │   ├── validators.js
│   │   └── helpers.js
│   ├── config/              # Configuration files
│   │   ├── database.js
│   │   └── socket.js
│   ├── app.js               # Express app setup
│   └── server.js            # Server entry point
├── tests/                   # Test files
├── docs/                    # API documentation
├── .env.example             # Environment template
├── package.json
└── README.md
```

---

## 🚀 Quick Start

### Prerequisites

- Node.js (v16+)
- MongoDB (local atau cloud)
- npm atau yarn

### 1. Clone Repository

```bash
git clone https://github.com/Peter-sour/clavox-backend.git
cd clavox-backend
```

### 2. Install Dependencies

```bash
# Core dependencies
npm install

# Production dependencies
npm install express socket.io mongoose cors dotenv
npm install jsonwebtoken bcryptjs
npm install express-validator express-rate-limit
npm install morgan helmet compression

# Development dependencies  
npm install --save-dev nodemon concurrently
npm install --save-dev jest supertest
```

### 3. Environment Setup

Copy `.env.example` ke `.env` dan isi konfigurasi:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database
MONGODB_URI=mongodb://localhost:27017/clavox
# atau MongoDB Atlas
# MONGODB_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/clavox

# JWT Secret
JWT_SECRET=your_super_secret_jwt_key_here
JWT_EXPIRE=7d

# OTP Service (Twilio example)
TWILIO_ACCOUNT_SID=your_twilio_account_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_PHONE_NUMBER=your_twilio_phone_number

# CORS Origin
FRONTEND_URL=http://localhost:5173

# Rate Limiting
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

### 4. Database Setup

```bash
# Pastikan MongoDB running
# Local: mongod
# Atlas: sudah otomatis online

# Jalankan seeder (optional)
npm run seed
```

### 5. Development Server

```bash
npm run dev
```

Server akan berjalan di `http://localhost:5000`

---

## 📡 API Endpoints

### Authentication

```http
POST /api/auth/send-otp
Content-Type: application/json

{
  "phoneNumber": "+6281234567890"
}
```

```http
POST /api/auth/verify-otp
Content-Type: application/json

{
  "phoneNumber": "+6281234567890",
  "otp": "123456"
}
```

```http
POST /api/auth/refresh-token
Authorization: Bearer <refresh_token>
```

### Chat Management

```http
GET /api/chats
Authorization: Bearer <access_token>

POST /api/chats
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "participants": ["userId1", "userId2"],
  "type": "private"
}
```

### Messages

```http
GET /api/chats/:chatId/messages?page=1&limit=20
Authorization: Bearer <access_token>

POST /api/chats/:chatId/messages
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "content": "Hello world!",
  "type": "text"
}
```

### User Management

```http
GET /api/users/profile
Authorization: Bearer <access_token>

PUT /api/users/profile
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "John Doe",
  "avatar": "avatar_url"
}
```

---

## 🔌 Socket.IO Events

### Client -> Server

```javascript
// Join user to socket
socket.emit('join_user', { userId, token });

// Join chat room
socket.emit('join_chat', { chatId });

// Send message
socket.emit('send_message', {
  chatId,
  content,
  type: 'text'
});

// Typing indicator
socket.emit('typing_start', { chatId });
socket.emit('typing_stop', { chatId });
```

### Server -> Client

```javascript
// New message received
socket.on('new_message', (message) => {
  console.log('New message:', message);
});

// User typing
socket.on('user_typing', ({ userId, chatId }) => {
  console.log(`User ${userId} is typing in ${chatId}`);
});

// User online/offline
socket.on('user_status', ({ userId, status }) => {
  console.log(`User ${userId} is ${status}`);
});

// Connection events
socket.on('connect', () => {
  console.log('Connected to server');
});

socket.on('disconnect', () => {
  console.log('Disconnected from server');
});
```

---

## 🧑‍💻 Available Scripts

```bash
npm run dev          # Development server dengan nodemon
npm run start        # Production server
npm run test         # Run tests
npm run test:watch   # Run tests in watch mode
npm run seed         # Database seeding
npm run migrate      # Database migration
npm run build        # Build untuk production (jika ada transpiling)
npm run lint         # ESLint check
npm run lint:fix     # Fix ESLint issues
```

---

## 🗄️ Database Schema

### User Model

```javascript
{
  _id: ObjectId,
  phoneNumber: String, // Unique, required
  name: String,
  avatar: String,
  isVerified: Boolean,
  lastSeen: Date,
  isOnline: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### Chat Model

```javascript
{
  _id: ObjectId,
  participants: [ObjectId], // User IDs
  type: String, // 'private' | 'group'
  name: String, // For group chats
  avatar: String, // For group chats
  lastMessage: ObjectId,
  createdAt: Date,
  updatedAt: Date
}
```

### Message Model

```javascript
{
  _id: ObjectId,
  chatId: ObjectId,
  senderId: ObjectId,
  content: String,
  type: String, // 'text' | 'image' | 'file' | 'voice'
  metadata: Object, // File info, image dimensions, etc.
  isRead: Boolean,
  readBy: [{ userId: ObjectId, readAt: Date }],
  createdAt: Date,
  updatedAt: Date
}
```

### OTP Model

```javascript
{
  _id: ObjectId,
  phoneNumber: String,
  otp: String,
  expiresAt: Date,
  isUsed: Boolean,
  attempts: Number,
  createdAt: Date
}
```

---

## 🔧 Middleware & Services

### Authentication Middleware

```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');

const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Access denied' });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({ error: 'Invalid token' });
    }
    req.user = user;
    next();
  });
};
```

### OTP Service

```javascript
// services/otpService.js
const generateOTP = () => {
  return Math.floor(100000 + Math.random() * 900000).toString();
};

const sendOTP = async (phoneNumber, otp) => {
  // Integration dengan Twilio, atau SMS gateway lainnya
  // Implementation sesuai provider yang dipilih
};
```

---

## 🛡️ Security Features

### Rate Limiting

```javascript
const rateLimit = require('express-rate-limit');

const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // limit each IP to 5 requests per windowMs
  message: 'Too many authentication attempts'
});
```

### Input Validation

```javascript
const { body, validationResult } = require('express-validator');

const validatePhoneNumber = [
  body('phoneNumber')
    .isMobilePhone()
    .withMessage('Valid phone number required'),
  
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    next();
  }
];
```

---

## ☁️ Deployment

### Environment Setup

```bash
# Production environment variables
NODE_ENV=production
PORT=5000
MONGODB_URI=mongodb+srv://...
JWT_SECRET=very_secure_secret_key
```

### Docker Setup

```dockerfile
FROM node:16-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 5000

CMD ["npm", "start"]
```

### Docker Compose

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "5000:5000"
    environment:
      - NODE_ENV=production
      - MONGODB_URI=mongodb://mongo:27017/clavox
    depends_on:
      - mongo
  
  mongo:
    image: mongo:5
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db

volumes:
  mongo_data:
```

---

## 🧪 Testing

### Unit Tests

```bash
npm test
```

### API Testing dengan Jest

```javascript
// tests/auth.test.js
const request = require('supertest');
const app = require('../src/app');

describe('Authentication', () => {
  test('POST /api/auth/send-otp', async () => {
    const response = await request(app)
      .post('/api/auth/send-otp')
      .send({ phoneNumber: '+6281234567890' });
    
    expect(response.status).toBe(200);
    expect(response.body).toHaveProperty('message');
  });
});
```

---

## 📊 Monitoring & Logging

### Winston Logger

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' })
  ]
});
```

---

## 📌 Roadmap Backend

- [ ] End-to-End Encryption
- [ ] File Upload dengan Cloud Storage
- [ ] Voice Message Support  
- [ ] Push Notification Service
- [ ] Message Reactions
- [ ] User Blocking/Reporting
- [ ] Admin Dashboard API
- [ ] Analytics & Metrics
- [ ] Redis Caching
- [ ] Microservices Architecture

---

## 🐛 Troubleshooting

### Common Issues

1. **MongoDB Connection Error**
   ```bash
   # Check MongoDB status
   mongod --version
   # atau untuk service
   systemctl status mongod
   ```

2. **Socket.IO CORS Issues**
   ```javascript
   // Pastikan CORS origin sesuai
   const io = new Server(server, {
     cors: {
       origin: process.env.FRONTEND_URL,
       methods: ["GET", "POST"]
     }
   });
   ```

3. **JWT Token Issues**
   ```bash
   # Generate new JWT secret
   node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
   ```

### Debug Mode

```bash
# Enable debug logging
DEBUG=express:* npm run dev

# Socket.IO debug
DEBUG=socket.io:* npm run dev
```

---

## 🤝 Contributing

1. Fork repository ini
2. Buat feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buka Pull Request

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 Acknowledgments

- [Express.js](https://expressjs.com/) - Web Framework
- [Socket.IO](https://socket.io/) - Real-time Engine
- [MongoDB](https://mongodb.com/) - Database
- [Mongoose](https://mongoosejs.com/) - ODM
- [JWT](https://jwt.io/) - Authentication

---

## 💡 Support

Jika project ini membantu, jangan lupa beri ⭐ di repo ini!

**Link Repository:**
- Backend: [Clavox Backend](https://github.com/Peter-sour/clavox-backend)  
- Frontend: [Clavox Frontend](https://github.com/Peter-sour/clavox-frontend)

---

**Made with ❤️ by [Peter-sour](https://github.com/Peter-sour)**
