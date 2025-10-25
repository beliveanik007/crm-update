# MySQL Database Setup Guide for CRM System

## Prerequisites

1. **MySQL Server** (v8.0 or higher)
2. **Node.js** (v16 or higher)
3. **npm** or **yarn**

## Step 1: Install MySQL

### Windows
1. Download MySQL Installer from [mysql.com](https://dev.mysql.com/downloads/installer/)
2. Run the installer and follow the setup wizard
3. Remember the root password you set

### macOS
```bash
brew install mysql
brew services start mysql
```

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install mysql-server
sudo systemctl start mysql
sudo systemctl enable mysql
```

## Step 2: Set Up Database

1. **Access MySQL:**
   ```bash
   mysql -u root -p
   ```

2. **Create Database:**
   ```sql
   CREATE DATABASE crm_database;
   USE crm_database;
   ```

3. **Run Schema:**
   ```bash
   mysql -u root -p crm_database < backend/database/schema.sql
   ```

## Step 3: Configure Backend

1. **Navigate to backend folder:**
   ```bash
   cd backend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Set up environment variables:**
   ```bash
   cp env.example .env
   ```

4. **Edit `.env` file:**
   ```env
   DB_HOST=localhost
   DB_USER=root
   DB_PASSWORD=your_mysql_password
   DB_NAME=crm_database
   DB_PORT=3306
   
   PORT=5000
   NODE_ENV=development
   JWT_SECRET=your_jwt_secret_key_here
   CORS_ORIGIN=http://localhost:5173
   ```

## Step 4: Start Backend Server

```bash
npm run dev
```

The server should start on port 5000 and connect to MySQL.

## Step 5: Test API

1. **Health Check:**
   ```bash
   curl http://localhost:5000/api/health
   ```

2. **Test Database Connection:**
   - Check console logs for "✅ MySQL database connected successfully"

## Step 6: Update Frontend

The frontend has been updated to use the real API instead of mock data. The main changes are:

1. **API Service** (`src/services/api.ts`) - Handles all backend communication
2. **Dashboard** - Now fetches real statistics from MySQL
3. **Clients** - CRUD operations now use real database
4. **Projects** - Ready for real data integration

## Troubleshooting

### Database Connection Issues
- Verify MySQL service is running
- Check credentials in `.env`
- Ensure database exists
- Check firewall settings

### Port Issues
- Change port in `.env` if 5000 is busy
- Kill process using the port: `npx kill-port 5000`

### CORS Issues
- Verify `CORS_ORIGIN` matches frontend URL
- Frontend should run on `http://localhost:5173`

## Default Login

For testing:
- **Email:** admin@crm.com
- **Password:** admin123

## Next Steps

1. **Complete remaining API routes:**
   - Invoices
   - Payments
   - Expenses
   - Tasks
   - Users

2. **Add authentication middleware** to protect routes

3. **Implement file uploads** for attachments

4. **Add data validation** and sanitization

5. **Set up production deployment**

## API Endpoints Available

- `GET /api/health` - Health check
- `POST /api/auth/login` - User login
- `GET /api/auth/profile` - User profile
- `GET /api/clients` - List clients
- `POST /api/clients` - Create client
- `PUT /api/clients/:id` - Update client
- `DELETE /api/clients/:id` - Delete client
- `GET /api/projects` - List projects
- `GET /api/dashboard/overview` - Dashboard statistics

## Database Schema

The system includes these main tables:
- **users** - User accounts
- **clients** - Client information
- **projects** - Project details
- **invoices** - Invoice records
- **payments** - Payment transactions
- **expenses** - Expense tracking
- **tasks** - Task management

All tables have proper foreign key relationships and timestamps.
