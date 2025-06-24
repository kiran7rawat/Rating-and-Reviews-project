# Setup Guide - ProductReview Application

This guide provides step-by-step instructions for setting up the ProductReview application based on the current project configuration.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v16.0.0 or higher)
  - Download from [nodejs.org](https://nodejs.org/)
  - Verify installation: `node --version`
- **npm** (comes with Node.js)
  - Verify installation: `npm --version`

## Development Setup

### 1. Project Installation

```bash
# Navigate to project directory
cd ratings-review-system

# Install all dependencies
npm install
```

### 2. Project Structure Overview

The project uses the following structure that's already configured:

```
├── src/                    # Frontend React/TypeScript code
│   ├── components/         # React components
│   ├── types/             # TypeScript definitions
│   ├── config/            # Configuration files
│   └── App.tsx            # Main application
├── server/                # Backend Express server
│   ├── index.js           # Main server file
│   └── uploads/           # File upload directory (auto-created)
├── public/                # Static assets
└── package.json           # Dependencies and scripts
```

### 3. Available Scripts

The project is configured with these npm scripts:

```bash
# Start both frontend and backend (recommended for development)
npm run dev

# Start only frontend (React/Vite on port 5173)
npm run dev:frontend

# Start only backend (Express on port 3001)
npm run dev:backend

# Build frontend for production
npm run build

# Preview production build
npm run preview

# Run ESLint for code quality
npm run lint
```

### 4. Start Development

```bash
# Start the complete application
npm run dev
```

This command will:
- Start the Express backend server on `http://localhost:3001`
- Start the Vite frontend development server on `http://localhost:5173`
- Enable hot reloading for both servers using concurrently

### 5. Verify Setup

1. **Backend API**: Open `http://localhost:3001/api/products`
   - Should return JSON with 6 sample products

2. **Frontend Application**: Open `http://localhost:5173`
   - Should display the ProductReview interface with product cards

## Configuration Details

### Current Technology Stack

**Frontend:**
- React 18 with TypeScript
- Vite for development and building
- Tailwind CSS for styling
- Lucide React for icons

**Backend:**
- Node.js with Express
- Multer for file uploads
- CORS for cross-origin requests
- UUID for unique identifiers

### Port Configuration

The application uses these default ports:
- **Frontend**: 5173 (Vite default)
- **Backend**: 3001 (configured in server/index.js)

### File Upload Configuration

Current settings in the project:
- **Upload Directory**: `server/uploads/` (auto-created)
- **File Size Limit**: 5MB per file
- **File Types**: Images only (jpg, png, gif, etc.)
- **Max Files per Review**: 3 photos

### API Configuration

The frontend is configured to connect to the backend via:
- **Base URL**: `http://localhost:3001` (defined in `src/config/index.ts`)
- **API Endpoints**: All prefixed with `/api/`

## Development Workflow

### 1. Making Changes

**Frontend Changes:**
- Edit files in `src/` directory
- Changes auto-reload in browser
- TypeScript compilation happens automatically

**Backend Changes:**
- Edit files in `server/` directory
- Server auto-restarts with nodemon
- API changes reflect immediately

### 2. Adding Dependencies

**Frontend Dependencies:**
```bash
npm install package-name
```

**Backend Dependencies:**
```bash
npm install package-name
```

Dependencies are automatically available to both frontend and backend.

### 3. File Uploads

The server automatically:
- Creates `server/uploads/` directory if it doesn't exist
- Handles file validation and storage
- Serves uploaded files at `/uploads/` endpoint

## Data Storage

### Current Implementation

The application currently uses:
- **In-memory storage** for products and reviews
- **Local file system** for uploaded images
- **Sample data** with 6 pre-loaded products

### Data Persistence

**Important**: Data is reset when the server restarts since it uses in-memory storage. This is suitable for development and testing.

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# If port 3001 is busy, find and kill the process
lsof -i :3001
kill -9 <PID>

# Or if port 5173 is busy
lsof -i :5173
kill -9 <PID>
```

#### Dependencies Issues
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

#### Upload Directory Issues
The server automatically creates the uploads directory, but if you encounter permissions issues:
```bash
# Ensure proper permissions
mkdir -p server/uploads
chmod 755 server/uploads
```

### Verification Steps

1. **Check if both servers are running:**
   - Look for "Server running on http://localhost:3001" in terminal
   - Look for "Local: http://localhost:5173" in terminal

2. **Test API connectivity:**
   ```bash
   curl http://localhost:3001/api/products
   ```

3. **Check browser console:**
   - Open Developer Tools (F12)
   - Look for any error messages in Console tab

## Environment-Specific Notes

### Windows Users
The project uses `concurrently` which works cross-platform, so no additional configuration needed.

### macOS/Linux Users
All commands should work as-is with the current configuration.

## Production Considerations

For production deployment, you would need to:

1. **Build the frontend:**
   ```bash
   npm run build
   ```

2. **Serve static files** from the `dist/` directory

3. **Use a process manager** like PM2 for the backend

4. **Configure a reverse proxy** like Nginx

5. **Set up persistent database** instead of in-memory storage

6. **Configure file storage** (cloud storage for uploaded files)

However, the current setup is optimized for development and testing purposes.

## Getting Help

If you encounter issues:

1. Ensure all prerequisites are installed
2. Check that both servers start without errors
3. Verify ports 3001 and 5173 are available
4. Check browser console for frontend errors
5. Check terminal output for backend errors

The application includes comprehensive error handling and validation, so most issues will be clearly reported in the console or browser.