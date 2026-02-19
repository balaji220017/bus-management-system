# Quick Setup Guide

## Step 1: Install Dependencies

```bash
cd bus-management-system
npm install
```

## Step 2: Configure Environment

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Edit `.env` and update the `JWT_SECRET` with a secure random string.

## Step 3: Initialize Database

```bash
# Generate Prisma Client
npm run db:generate

# Create database and tables
npm run db:push
```

## Step 4: (Optional) Seed Sample Data

If you want to populate the database with sample data:

```bash
# Install tsx if not already installed
npm install -D tsx

# Run seed script
npx tsx scripts/seed.ts
```

This will create:
- Admin user (email: admin@bussystem.com, password: admin123)
- Sample buses
- Sample routes

## Step 5: Start Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Testing the Application

1. **Register a new user** at `/register`
2. **Login** at `/login`
3. **Search for routes** at `/book`
4. **Book a ticket** by selecting a route and seats
5. **View bookings** at `/dashboard`
6. **Track buses** at `/track`

## Admin Access

To access admin features:
1. Use the seed script to create an admin user, OR
2. Manually update a user's role to `ADMIN` in the database using Prisma Studio:

```bash
npm run db:studio
```

Then navigate to the User table and change a user's role to `ADMIN`.

## Troubleshooting

### Database Issues
- Make sure `DATABASE_URL` in `.env` points to a valid path
- Run `npm run db:push` again if schema changes

### Authentication Issues
- Clear browser localStorage if tokens are invalid
- Make sure `JWT_SECRET` is set in `.env`

### Port Already in Use
- Change the port: `npm run dev -- -p 3001`

## Next Steps

- Integrate payment gateway (Stripe/Razorpay)
- Add real-time bus tracking with Socket.io
- Implement email notifications
- Add QR code generation for tickets
- Integrate Google Maps for visual tracking
