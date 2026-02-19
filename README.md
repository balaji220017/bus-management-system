# Smart Web-Based Bus Management and Digital Ticketing System

A comprehensive web application for managing bus operations, digital ticket booking, and real-time bus tracking.

## 🚀 Features

### Core Functionality
- **Digital Ticket Booking**: Book tickets online with seat selection
- **Real-Time Bus Tracking**: Track bus locations in real-time
- **Route Management**: View and search available routes
- **User Authentication**: Secure login and registration system
- **Booking Management**: View booking history and manage tickets
- **Admin Dashboard**: Manage buses, routes, and view statistics

### Key Benefits
- ✅ Eliminates long waiting times
- ✅ Reduces manual errors in ticket issuing
- ✅ Provides real-time bus tracking
- ✅ Maintains digital passenger and revenue records
- ✅ Improves communication between passengers and management
- ✅ Reduces paper wastage
- ✅ Increases transparency

## 🛠️ Tech Stack

- **Frontend**: Next.js 14 (React), TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes
- **Database**: SQLite (via Prisma ORM)
- **Authentication**: JWT (JSON Web Tokens)
- **Real-time**: Socket.io (ready for implementation)

## 📋 Prerequisites

- Node.js 18+ installed
- npm or yarn package manager

## 🚦 Getting Started

### 1. Install Dependencies

```bash
npm install
```

### 2. Set Up Environment Variables

Create a `.env` file in the root directory:

```env
DATABASE_URL="file:./dev.db"
JWT_SECRET="your-secret-key-change-in-production"
```

### 3. Set Up Database

```bash
# Generate Prisma Client
npm run db:generate

# Push schema to database
npm run db:push
```

### 4. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## 📁 Project Structure

```
bus-management-system/
├── app/
│   ├── api/              # API routes
│   │   ├── auth/         # Authentication endpoints
│   │   ├── bookings/     # Booking endpoints
│   │   ├── buses/        # Bus management endpoints
│   │   └── routes/       # Route endpoints
│   ├── admin/            # Admin pages
│   ├── book/             # Booking pages
│   ├── dashboard/        # User dashboard
│   ├── login/            # Login page
│   ├── register/         # Registration page
│   ├── routes/           # Routes listing
│   ├── track/            # Bus tracking page
│   └── page.tsx          # Homepage
├── components/           # React components
├── lib/                  # Utility functions
│   ├── auth.ts          # Authentication utilities
│   └── prisma.ts        # Prisma client
├── prisma/
│   └── schema.prisma    # Database schema
└── public/              # Static assets
```

## 🗄️ Database Schema

The system uses the following main entities:

- **User**: Passengers, admins, and drivers
- **Bus**: Bus information and status
- **Route**: Bus routes with origin, destination, and pricing
- **Booking**: Passenger bookings
- **Ticket**: Digital tickets linked to bookings

## 🔐 Authentication

The system uses JWT-based authentication:
- Register at `/register`
- Login at `/login`
- Protected routes require authentication token
- Token stored in localStorage (client-side)

## 📱 Key Pages

- **Home** (`/`): Landing page with features overview
- **Book Ticket** (`/book`): Search and book tickets
- **Track Bus** (`/track`): Real-time bus tracking
- **Routes** (`/routes`): View all available routes
- **Dashboard** (`/dashboard`): User booking history
- **Admin** (`/admin`): Admin management panel

## 🔧 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user

### Routes
- `GET /api/routes` - Get all routes (with optional query params: origin, destination)

### Bookings
- `GET /api/bookings` - Get user bookings (requires auth)
- `POST /api/bookings` - Create new booking (requires auth)

### Buses
- `GET /api/buses` - Get all buses
- `GET /api/buses/[id]/location` - Get bus location
- `PUT /api/buses/[id]/location` - Update bus location

## 🎨 Features Implementation Status

- ✅ User Registration & Login
- ✅ Route Search & Display
- ✅ Seat Selection & Booking
- ✅ Booking History
- ✅ Bus Tracking (API ready)
- ✅ Admin Dashboard (basic)
- ⚠️ Real-time updates (Socket.io ready, needs implementation)
- ⚠️ Payment integration (structure ready)
- ⚠️ Email notifications (to be implemented)
- ⚠️ QR Code generation (structure ready)

## 🚧 Future Enhancements

1. **Payment Integration**: Integrate payment gateways (Stripe, Razorpay)
2. **Real-time Updates**: Implement Socket.io for live bus tracking
3. **Email Notifications**: Send booking confirmations via email
4. **QR Code Tickets**: Generate QR codes for tickets
5. **Mobile App**: React Native mobile application
6. **Advanced Analytics**: Revenue reports and analytics dashboard
7. **SMS Notifications**: Send SMS for booking confirmations
8. **Map Integration**: Google Maps/Mapbox for visual bus tracking

## 🧪 Development Commands

```bash
# Development
npm run dev          # Start development server

# Database
npm run db:generate  # Generate Prisma Client
npm run db:push      # Push schema changes
npm run db:studio    # Open Prisma Studio

# Production
npm run build        # Build for production
npm run start        # Start production server
```

## 🌐 Hosting (Frontend + Backend + Database)

To host the full stack (web app, API, and database) in the cloud:

1. **Backend + Database:** Deploy to [Railway](https://railway.app) with PostgreSQL (see [DEPLOYMENT.md](./DEPLOYMENT.md)).
2. **Frontend:** Deploy to [Vercel](https://vercel.com) and set `NEXT_PUBLIC_API_URL` to your Railway backend URL.

Detailed steps, environment variables, and troubleshooting are in **[DEPLOYMENT.md](./DEPLOYMENT.md)**.

## 📝 Notes

- The database uses SQLite for easy setup. For production, use PostgreSQL (see DEPLOYMENT.md).
- JWT secret should be changed in production environment.
- Real-time tracking requires Socket.io server setup (structure ready).
- Map integration requires API keys from Google Maps or Mapbox.

## 🤝 Contributing

This is a project template. Feel free to extend it with additional features:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📄 License

This project is open source and available for educational purposes.

## 👥 Support

For issues or questions, please create an issue in the repository.

---

**Built with ❤️ using Next.js, TypeScript, and Tailwind CSS**
