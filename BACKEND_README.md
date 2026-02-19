# Backend API Server

This is the standalone Express.js backend server for the Bus Management System.

## 🚀 Quick Start

### 1. Install Dependencies

```bash
npm install
```

### 2. Configure Environment

Ensure your `.env` file is set up (see `DATABASE_SETUP.md`):

```env
DATABASE_URL="file:./dev.db"
JWT_SECRET="your-secret-key"
PORT=5000
FRONTEND_URL="http://localhost:3000"
```

### 3. Initialize Database

```bash
npm run db:generate
npm run db:push
```

### 4. Start Backend Server

```bash
# Development mode (with hot reload)
npm run dev:backend

# Or start both frontend and backend together
npm run dev:all
```

The backend will run on `http://localhost:5000`

## 📁 Project Structure

```
server/
├── index.ts              # Main server file
├── config/
│   └── database.ts       # Prisma client configuration
├── routes/
│   ├── auth.ts          # Authentication routes
│   ├── bookings.ts     # Booking management routes
│   ├── buses.ts         # Bus management routes
│   ├── routes.ts        # Route management routes
│   └── admin.ts         # Admin routes
├── middleware/
│   └── auth.ts          # Authentication middleware
└── utils/
    └── auth.ts          # Auth utility functions
```

## 🔌 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user (protected)

### Bookings
- `GET /api/bookings` - Get user bookings (protected)
- `POST /api/bookings` - Create new booking (protected)
- `GET /api/bookings/:id` - Get booking by ID (protected)
- `PATCH /api/bookings/:id/cancel` - Cancel booking (protected)

### Buses
- `GET /api/buses` - Get all buses
- `GET /api/buses/:id` - Get bus by ID
- `GET /api/buses/:id/location` - Get bus location
- `PUT /api/buses/:id/location` - Update bus location

### Routes
- `GET /api/routes` - Get all routes (query params: origin, destination)
- `GET /api/routes/:id` - Get route by ID

### Admin (Protected - Admin only)
- `GET /api/admin/stats` - Get dashboard statistics
- `POST /api/admin/buses` - Create new bus
- `PUT /api/admin/buses/:id` - Update bus
- `DELETE /api/admin/buses/:id` - Delete bus
- `POST /api/admin/routes` - Create new route
- `PUT /api/admin/routes/:id` - Update route
- `DELETE /api/admin/routes/:id` - Cancel route
- `GET /api/admin/bookings` - Get all bookings

## 🔐 Authentication

All protected routes require a Bearer token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

## 📝 Example Requests

### Register User
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "password123",
    "name": "John Doe",
    "phone": "1234567890"
  }'
```

### Login
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "password123"
  }'
```

### Create Booking (Protected)
```bash
curl -X POST http://localhost:5000/api/bookings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <your-token>" \
  -d '{
    "routeId": "route-id",
    "busId": "bus-id",
    "seatNumber": 5,
    "travelDate": "2024-12-25"
  }'
```

### Get Bus Location
```bash
curl http://localhost:5000/api/buses/bus-id/location
```

## 🛠️ Development

### Run in Development Mode
```bash
npm run dev:backend
```

This uses `tsx watch` for hot reloading.

### Build for Production
```bash
npm run build:backend
```

### Run Production Build
```bash
npm run start:backend
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_URL` | Database connection string | `file:./dev.db` |
| `JWT_SECRET` | Secret key for JWT tokens | Required |
| `PORT` | Server port | `5000` |
| `FRONTEND_URL` | Frontend URL for CORS | `http://localhost:3000` |
| `NODE_ENV` | Environment mode | `development` |

### CORS Configuration

The server is configured to accept requests from the frontend URL specified in `FRONTEND_URL`. Update this in production.

## 🗄️ Database

The backend uses Prisma ORM. See `DATABASE_SETUP.md` for database configuration options.

## 🧪 Testing

Health check endpoint:
```bash
curl http://localhost:5000/health
```

Response:
```json
{
  "status": "ok",
  "message": "Bus Management API is running",
  "timestamp": "2024-02-19T12:00:00.000Z",
  "database": "connected"
}
```

## 📦 Dependencies

- **express** - Web framework
- **cors** - CORS middleware
- **dotenv** - Environment variables
- **@prisma/client** - Database ORM
- **jsonwebtoken** - JWT authentication
- **bcryptjs** - Password hashing
- **zod** - Schema validation

## 🚨 Error Handling

The server includes comprehensive error handling:
- Validation errors (400)
- Authentication errors (401)
- Authorization errors (403)
- Not found errors (404)
- Server errors (500)

All errors return JSON format:
```json
{
  "error": "Error message",
  "details": {} // Optional, for validation errors
}
```

## 🔄 Integration with Frontend

The frontend (Next.js) can connect to this backend by updating API calls:

```typescript
// Instead of: /api/auth/login
// Use: http://localhost:5000/api/auth/login

const response = await fetch('http://localhost:5000/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email, password }),
});
```

## 📚 Additional Resources

- [Express.js Documentation](https://expressjs.com/)
- [Prisma Documentation](https://www.prisma.io/docs)
- [JWT Authentication](https://jwt.io/)
