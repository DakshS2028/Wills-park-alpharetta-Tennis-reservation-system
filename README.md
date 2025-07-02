# Wills Park Tennis Court Booking System

A comprehensive tennis court reservation system for the City of Alpharetta's Wills Park facility. This full-stack application allows users to book tennis courts, view availability, manage reservations, and includes administrative features.

## Features

### User Features
- **Authentication System**: Secure user login with JWT tokens
- **Court Booking**: Reserve tennis courts with real-time availability
- **Pricing Calculator**: Automatic pricing based on resident status and membership
- **Reservation Management**: View and manage personal reservations
- **Real-time Notifications**: System notifications for important updates
- **Responsive Design**: Mobile-friendly interface using Tailwind CSS

### Admin Features
- **Dashboard Analytics**: Overview of reservations, revenue, and users
- **User Management**: Update user status and membership information
- **Court Maintenance**: Toggle court availability for maintenance
- **Notification System**: Send system-wide notifications to users
- **Comprehensive Reports**: Detailed analytics and reporting

## Technology Stack

- **Frontend**: React 19, Tailwind CSS, Craco
- **Backend**: FastAPI, Python 3.11
- **Database**: MongoDB
- **Authentication**: JWT tokens
- **Payment Processing**: Stripe integration (demo mode)
- **Process Management**: Supervisor


## Prerequisites

### For Mac Users
•⁠  ⁠*Homebrew*: Install from https://brew.sh

•⁠  ⁠*Python 3.11+*: ⁠ 
```
brew install python@3.11 ⁠
```
•⁠  ⁠*Node.js 18+*: ⁠ 
```
brew install node ⁠
```
•⁠  ⁠*MongoDB*: ⁠ 
```
brew tap mongodb/brew && brew install mongodb-community ⁠
```
•⁠  ⁠*Yarn*: ⁠ 
```
brew install yarn ⁠
```
# Mac Development Setup

# 1. Clone and Navigate
```bash
git clone [repository-url]
cd /app
```
# 2. Install Dependencies (Do the prerequisites section)
⁠ 
### Install Homebrew (if not already installed)
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
### Install required packages
```
brew install python@3.11 node yarn
brew tap mongodb/brew && brew install mongodb-community
```

# 3.⁠ ⁠Setup Project
⁠
## 1. Navigate to the project in your files
```
cd /app
```
## 2. Navigate to the Backend and install backend dependencies 
```
cd backend
```
## 3. To install backend dependencies, try one of these commands (in order of preference):
```
pip3 install -r requirements.txt
```
### OR if pip3 doesn't work:
```
python3 -m pip install -r requirements.txt
```
### OR if you have brew python:
```
/usr/local/bin/python3 -m pip install -r requirements.txt
```

## 4. Install frontend dependencies  
```
cd ../frontend && yarn install
```
 ⁠

# 4.⁠ Start Services in 3 Terminal Windows

## Terminal 1: MongoDB
```
brew services start mongodb/brew/mongodb-community
```
## Terminal 2: Backend API
```
cd /app/backend && python3 server.py
```
## Terminal 3: Frontend App
```
cd /app/frontend && yarn start
```
# 5. Access the Application
```

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:8001
- **API Documentation**: http://localhost:8001/docs
```

## Service Management

### Supervisor Commands
```bash
# Restart all services
sudo supervisorctl restart all

# Restart individual services
sudo supervisorctl restart backend
sudo supervisorctl restart frontend

# Stop services
sudo supervisorctl stop all

# View service status
sudo supervisorctl status

# View logs
tail -f /var/log/supervisor/backend.err.log
tail -f /var/log/supervisor/frontend.err.log
```


##  Testing the Application

### 1. Frontend-Backend Communication Test
```bash
# Test backend API directly
curl http://localhost:8001/api/courts

# Should return JSON with court data
```

### 2. Authentication Test
```bash
# Test login endpoint
curl -X POST http://localhost:8001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "membermock", "password": "trial123"}'

# Should return JWT token and user data
```

### 3. Frontend Access Test
- Navigate to http://localhost:3000
- Login with test credentials
- Verify all features work: booking, availability, reservations

## 🏗️ Application Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   React App     │    │   FastAPI       │    │   MongoDB       │
│   (Frontend)    │◄──►│   (Backend)     │◄──►│   (Database)    │
│   Port 3000     │    │   Port 8001     │    │   Port 27017    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Key Components
- **Frontend**: React SPA with Tailwind CSS
- **Backend**: RESTful API with FastAPI
- **Database**: MongoDB with collections for users, courts, reservations
- **Authentication**: JWT-based authentication system
- **Payment**: Stripe integration for payment processing

## API Endpoints

### Public Endpoints
- `POST /api/auth/login` - User authentication

### Protected Endpoints
- `GET /api/courts` - List all courts
- `GET /api/courts/availability` - Check court availability
- `POST /api/reservations` - Create new reservation
- `GET /api/reservations/my` - Get user's reservations
- `GET /api/notifications` - Get user notifications

### Admin Endpoints
- `GET /api/admin/reservations` - All reservations
- `GET /api/admin/users` - All users
- `GET /api/admin/analytics` - System analytics
- `POST /api/admin/notifications` - Send notifications

## Environment Variables

### Backend (.env)
```env
MONGO_URL=mongodb://localhost:27017/
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

### Frontend (.env)
```env
REACT_APP_BACKEND_URL=https://[your-backend-url]
WDS_SOCKET_PORT=443
```

## Troubleshooting

### Common Issues

#### Backend Not Starting
```bash
# Check logs
tail -f /var/log/supervisor/backend.err.log

# Common fixes:
# 1. Check MongoDB is running
# 2. Verify Python dependencies
# 3. Check port 8001 is available
```

#### Frontend Not Loading
```bash
# Check logs
tail -f /var/log/supervisor/frontend.err.log

# Common fixes:
# 1. Ensure dependencies installed with yarn (not npm)
# 2. Check port 3000 is available
# 3. Verify REACT_APP_BACKEND_URL is set
```

#### CORS Issues
- Ensure backend CORS middleware is properly configured
- Check that frontend is making requests to correct backend URL

#### Database Connection Issues
```bash
# Check MongoDB status
sudo supervisorctl status mongodb

# Check MongoDB connection
mongo --eval "db.adminCommand('ismaster')"
```

## Development Workflow

### Making Changes

1. **Backend Changes**:
   - Modify Python files in `/app/backend/`
   - Hot reload is enabled via uvicorn
   - Or restart: `sudo supervisorctl restart backend`

2. **Frontend Changes**:
   - Modify React files in `/app/frontend/src/`
   - Hot reload is enabled via Craco
   - Or restart: `sudo supervisorctl restart frontend`

3. **Database Changes**:
   - MongoDB data persists between restarts
   - Use MongoDB shell or GUI tools for direct database access

### Adding New Dependencies

#### Backend
```bash
cd /app/backend
pip install [package-name]
# Add to requirements.txt
echo "[package-name]==[version]" >> requirements.txt
```

#### Frontend
```bash
cd /app/frontend
yarn add [package-name]
# Automatically updates package.json
```

## Production Considerations

### Security
- Change the default JWT secret in production
- Use environment-specific Stripe keys
- Implement proper HTTPS
- Secure MongoDB with authentication

### Performance
- Implement database indexing
- Add caching layer (Redis)
- Optimize React bundle size
- Use a production-grade WSGI server

### Monitoring
- Set up logging aggregation
- Monitor API response times
- Track user activities
- Set up alerting for critical errors

##  Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

##  License

This project is developed for the City of Alpharetta's tennis court management system by Daksh Shah




**Last Updated**: July 2025
**Version**: 1.0.0
