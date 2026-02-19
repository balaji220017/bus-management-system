# Database Setup Guide

This guide explains how to configure the DATABASE_URL for different database systems.

## Database Options

### 1. SQLite (Default - Development)

**Best for:** Development, testing, small projects

**Setup:**
```env
DATABASE_URL="file:./dev.db"
```

**Pros:**
- No installation required
- Zero configuration
- Perfect for development
- File-based (easy to backup)

**Cons:**
- Not suitable for production
- Limited concurrent connections
- No advanced features

**Initialize:**
```bash
npm run db:push
```

---

### 2. PostgreSQL (Recommended for Production)

**Best for:** Production, large-scale applications

**Setup:**

1. Install PostgreSQL:
   - Windows: Download from [postgresql.org](https://www.postgresql.org/download/windows/)
   - macOS: `brew install postgresql`
   - Linux: `sudo apt-get install postgresql`

2. Create database:
```sql
CREATE DATABASE bus_management;
CREATE USER bus_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE bus_management TO bus_user;
```

3. Update `.env`:
```env
DATABASE_URL="postgresql://bus_user:your_password@localhost:5432/bus_management?schema=public"
```

4. Update Prisma schema (`prisma/schema.prisma`):
```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

5. Initialize:
```bash
npm run db:generate
npm run db:push
```

**Pros:**
- Production-ready
- Excellent performance
- Advanced features
- ACID compliance
- Great for concurrent users

**Cons:**
- Requires installation
- More complex setup

---

### 3. MySQL

**Best for:** Production, web applications

**Setup:**

1. Install MySQL:
   - Windows: Download from [mysql.com](https://dev.mysql.com/downloads/installer/)
   - macOS: `brew install mysql`
   - Linux: `sudo apt-get install mysql-server`

2. Create database:
```sql
CREATE DATABASE bus_management;
CREATE USER 'bus_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON bus_management.* TO 'bus_user'@'localhost';
FLUSH PRIVILEGES;
```

3. Update `.env`:
```env
DATABASE_URL="mysql://bus_user:your_password@localhost:3306/bus_management"
```

4. Update Prisma schema (`prisma/schema.prisma`):
```prisma
datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}
```

5. Initialize:
```bash
npm run db:generate
npm run db:push
```

**Pros:**
- Widely used
- Good performance
- Easy to manage
- Great tooling

**Cons:**
- Requires installation
- Some limitations compared to PostgreSQL

---

## Cloud Database Options

### PostgreSQL on Cloud

**Heroku Postgres:**
```env
DATABASE_URL="postgresql://user:pass@host:5432/dbname"
```

**Supabase:**
```env
DATABASE_URL="postgresql://postgres:password@db.xxxxx.supabase.co:5432/postgres"
```

**AWS RDS:**
```env
DATABASE_URL="postgresql://username:password@your-instance.xxxxx.us-east-1.rds.amazonaws.com:5432/bus_management"
```

**Railway:**
```env
DATABASE_URL="postgresql://postgres:password@containers-us-west-xxx.railway.app:5432/railway"
```

---

## Switching Databases

### From SQLite to PostgreSQL/MySQL

1. Update `prisma/schema.prisma`:
   - Change `provider = "sqlite"` to `provider = "postgresql"` or `provider = "mysql"`

2. Update `.env`:
   - Change `DATABASE_URL` to your new database connection string

3. Generate and push:
```bash
npm run db:generate
npm run db:push
```

**Note:** Data migration from SQLite to PostgreSQL/MySQL requires manual migration or use of Prisma Migrate.

---

## Environment-Specific Configuration

### Development
```env
DATABASE_URL="file:./dev.db"
NODE_ENV=development
```

### Production
```env
DATABASE_URL="postgresql://user:pass@host:5432/bus_management"
NODE_ENV=production
```

### Testing
```env
DATABASE_URL="file:./test.db"
NODE_ENV=test
```

---

## Troubleshooting

### Connection Issues

1. **Check database is running:**
   - PostgreSQL: `pg_isready`
   - MySQL: `mysqladmin ping`

2. **Verify credentials:**
   - Test connection manually
   - Check username/password

3. **Check firewall:**
   - Ensure port is open (5432 for PostgreSQL, 3306 for MySQL)

### Prisma Issues

1. **Reset database:**
```bash
npx prisma migrate reset
```

2. **View database:**
```bash
npm run db:studio
```

3. **Generate client:**
```bash
npm run db:generate
```

---

## Best Practices

1. **Never commit `.env` file** - Use `.env.example` as template
2. **Use different databases** for development and production
3. **Use connection pooling** for production databases
4. **Regular backups** - Especially for production
5. **Use migrations** instead of `db:push` in production

---

## Quick Start (SQLite)

For immediate development:

```bash
# 1. Ensure .env exists with:
DATABASE_URL="file:./dev.db"

# 2. Initialize database
npm run db:generate
npm run db:push

# 3. Start backend
npm run dev:backend
```

Your database is ready! 🎉
