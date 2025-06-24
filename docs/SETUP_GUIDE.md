# Setup Guide

This guide provides step-by-step instructions for setting up the ProductReview application in different environments.

## Table of Contents
1. [Development Setup](#development-setup)
2. [Production Setup](#production-setup)
3. [Docker Setup](#docker-setup)
4. [Troubleshooting](#troubleshooting)
5. [Configuration Options](#configuration-options)

## Development Setup

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v16.0.0 or higher)
  - Download from [nodejs.org](https://nodejs.org/)
  - Verify installation: `node --version`
- **npm** (comes with Node.js)
  - Verify installation: `npm --version`
- **Git** (optional, for cloning)
  - Download from [git-scm.com](https://git-scm.com/)

### Step-by-Step Installation

#### 1. Get the Source Code

**Option A: Clone from repository**
```bash
git clone <repository-url>
cd ratings-review-system
```

**Option B: Download and extract**
- Download the project ZIP file
- Extract to your desired directory
- Navigate to the project folder

#### 2. Install Dependencies

```bash
# Install all dependencies (frontend and backend)
npm install
```

This will install:
- Frontend dependencies (React, TypeScript, Tailwind CSS, etc.)
- Backend dependencies (Express, Multer, CORS, etc.)
- Development tools (ESLint, Vite, etc.)

#### 3. Verify Installation

Check that all dependencies are installed correctly:
```bash
# Check if node_modules directory exists
ls node_modules

# Verify package.json scripts
npm run
```

#### 4. Start Development Servers

```bash
# Start both frontend and backend servers
npm run dev
```

This command will:
- Start the backend server on `http://localhost:3001`
- Start the frontend development server on `http://localhost:5173`
- Enable hot reloading for both servers

#### 5. Verify Setup

1. **Check Backend**: Open `http://localhost:3001/api/products` in your browser
   - You should see JSON data with product information

2. **Check Frontend**: Open `http://localhost:5173` in your browser
   - You should see the ProductReview application with 6 sample products

### Alternative Development Commands

```bash
# Start only frontend
npm run dev:frontend

# Start only backend
npm run dev:backend

# Build for production
npm run build

# Preview production build
npm run preview

# Run linting
npm run lint
```

## Production Setup

### Prerequisites for Production

- **Node.js** (v16.0.0 or higher)
- **Process Manager** (PM2 recommended)
- **Reverse Proxy** (Nginx recommended)
- **SSL Certificate** (Let's Encrypt recommended)

### Step 1: Install PM2

```bash
# Install PM2 globally
npm install -g pm2
```

### Step 2: Build the Application

```bash
# Build the frontend
npm run build
```

### Step 3: Create Production Configuration

Create `ecosystem.config.js` for PM2:

```javascript
module.exports = {
  apps: [{
    name: 'productreview-backend',
    script: 'server/index.js',
    env: {
      NODE_ENV: 'production',
      PORT: 3001
    }
  }]
};
```

### Step 4: Start with PM2

```bash
# Start the backend with PM2
pm2 start ecosystem.config.js

# Save PM2 configuration
pm2 save

# Setup PM2 to start on boot
pm2 startup
```

### Step 5: Serve Frontend Files

**Option A: Using Nginx**

Create `/etc/nginx/sites-available/productreview`:

```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    # Serve frontend files
    location / {
        root /path/to/your/project/dist;
        try_files $uri $uri/ /index.html;
    }
    
    # Proxy API requests to backend
    location /api/ {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    
    # Serve uploaded files
    location /uploads/ {
        proxy_pass http://localhost:3001;
    }
}
```

Enable the site:
```bash
sudo ln -s /etc/nginx/sites-available/productreview /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

**Option B: Using Express to serve static files**

Modify `server/index.js` to serve the built frontend:

```javascript
// Add this after other middleware
app.use(express.static(path.join(__dirname, '../dist')));

// Add this before other routes
app.get('*', (req, res) => {
  if (req.path.startsWith('/api/') || req.path.startsWith('/uploads/')) {
    return next();
  }
  res.sendFile(path.join(__dirname, '../dist/index.html'));
});
```

## Docker Setup

### Create Dockerfile

```dockerfile
# Frontend build stage
FROM node:18-alpine AS frontend-build
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app

# Copy backend files
COPY server/ ./server/
COPY package*.json ./
RUN npm ci --only=production

# Copy built frontend
COPY --from=frontend-build /app/dist ./dist

# Create uploads directory
RUN mkdir -p server/uploads

# Expose port
EXPOSE 3001

# Start the application
CMD ["node", "server/index.js"]
```

### Create docker-compose.yml

```yaml
version: '3.8'
services:
  productreview:
    build: .
    ports:
      - "3001:3001"
    volumes:
      - ./server/uploads:/app/server/uploads
    environment:
      - NODE_ENV=production
    restart: unless-stopped
```

### Build and Run

```bash
# Build and start with Docker Compose
docker-compose up -d

# Or build and run manually
docker build -t productreview .
docker run -p 3001:3001 -v $(pwd)/server/uploads:/app/server/uploads productreview
```

## Troubleshooting

### Common Issues

#### 1. Port Already in Use

**Error**: `EADDRINUSE: address already in use :::3001`

**Solutions**:
```bash
# Find process using port 3001
lsof -i :3001

# Kill the process
kill -9 <PID>

# Or use a different port
PORT=3002 npm run dev:backend
```

#### 2. Permission Errors

**Error**: `EACCES: permission denied`

**Solutions**:
```bash
# Fix npm permissions
sudo chown -R $(whoami) ~/.npm

# Or use npx instead of global install
npx create-react-app my-app
```

#### 3. Module Not Found

**Error**: `Cannot find module 'xyz'`

**Solutions**:
```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

#### 4. Build Errors

**Error**: Build fails with TypeScript errors

**Solutions**:
```bash
# Check TypeScript configuration
npx tsc --noEmit

# Update dependencies
npm update

# Clear build cache
rm -rf dist .vite
```

### Environment-Specific Issues

#### Windows Users

```bash
# Use cross-env for environment variables
npm install --save-dev cross-env

# Update package.json scripts
"dev": "cross-env NODE_ENV=development concurrently \"npm run dev:frontend\" \"npm run dev:backend\""
```

#### macOS Users

```bash
# Install Xcode command line tools if needed
xcode-select --install
```

#### Linux Users

```bash
# Install build essentials if needed
sudo apt-get install build-essential
```

## Configuration Options

### Environment Variables

Create `.env` file in the project root:

```env
# Backend Configuration
PORT=3001
NODE_ENV=development

# Frontend Configuration
VITE_API_BASE_URL=http://localhost:3001

# File Upload Configuration
MAX_FILE_SIZE=5242880
MAX_FILES_PER_REVIEW=3
UPLOAD_DIR=server/uploads
```

### Customizing Ports

**Backend Port**:
```javascript
// In server/index.js
const PORT = process.env.PORT || 3001;
```

**Frontend Port**:
```javascript
// In vite.config.ts
export default defineConfig({
  server: {
    port: 5173
  }
});
```

### Database Integration

For production use, replace in-memory storage with a database:

```javascript
// Example with MongoDB
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/productreview');

// Example with PostgreSQL
const { Pool } = require('pg');
const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});
```

## Performance Optimization

### Frontend Optimization

```javascript
// In vite.config.ts
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          ui: ['lucide-react']
        }
      }
    }
  }
});
```

### Backend Optimization

```javascript
// Add compression middleware
const compression = require('compression');
app.use(compression());

// Add caching headers
app.use('/uploads', express.static('uploads', {
  maxAge: '1d'
}));
```

## Security Considerations

### Production Security

1. **Environment Variables**: Never commit sensitive data
2. **CORS**: Configure for specific domains only
3. **Rate Limiting**: Implement API rate limiting
4. **File Validation**: Strict file type and size validation
5. **Input Sanitization**: Sanitize all user inputs
6. **HTTPS**: Always use HTTPS in production

### Example Security Configuration

```javascript
// Rate limiting
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});
app.use('/api/', limiter);

// CORS for production
app.use(cors({
  origin: ['https://yourdomain.com'],
  credentials: true
}));
```

---

## Getting Help

If you encounter issues not covered in this guide:

1. Check the [Troubleshooting](#troubleshooting) section
2. Review the [Testing Guide](./TESTING_GUIDE.md)
3. Check the browser console for error messages
4. Verify all prerequisites are installed correctly
5. Try the setup process on a fresh system/container

For additional support, please open an issue in the project repository.