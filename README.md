# ProductReview - Rating & Review System

A modern, full-stack web application for product ratings and reviews built with React, TypeScript, Node.js, and Express.

![ProductReview Screenshot](https://images.pexels.com/photos/3184291/pexels-photo-3184291.jpeg?auto=compress&cs=tinysrgb&w=1200&h=400&fit=crop)

## Features

- **Product Catalog**: Browse and view detailed product information
- **Rating System**: 5-star rating system with visual feedback
- **Review Management**: Write detailed reviews with text, photos, and tags
- **Photo Uploads**: Support for multiple photo uploads per review (up to 3 photos, 5MB each)
- **Tag System**: Categorize reviews with custom tags
- **Real-time Statistics**: Dynamic calculation of average ratings and review counts
- **Responsive Design**: Mobile-first design with Tailwind CSS
- **User-friendly Interface**: Modern UI with smooth animations and transitions

## Tech Stack

### Frontend
- **React 18** with TypeScript
- **Tailwind CSS** for styling
- **Lucide React** for icons
- **Vite** for build tooling

### Backend
- **Node.js** with Express
- **Multer** for file uploads
- **CORS** for cross-origin requests
- **UUID** for unique identifiers

## Quick Start

### Prerequisites
- Node.js (v16 or higher)
- npm or yarn

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ratings-review-system
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start the development servers**
   ```bash
   npm run dev
   ```

   This command starts both the frontend (port 5173) and backend (port 3001) servers concurrently.

4. **Open your browser**
   Navigate to `http://localhost:5173` to view the application.

## Project Structure

```
├── src/                    # Frontend source code
│   ├── components/         # React components
│   │   ├── ProductCard.tsx
│   │   ├── RatingStars.tsx
│   │   ├── ReviewList.tsx
│   │   └── ReviewModal.tsx
│   ├── types/             # TypeScript type definitions
│   ├── config/            # Configuration files
│   ├── App.tsx            # Main application component
│   └── main.tsx           # Application entry point
├── server/                # Backend source code
│   ├── index.js           # Express server
│   └── uploads/           # File upload directory
├── public/                # Static assets
└── docs/                  # Documentation
```

## Available Scripts

- `npm run dev` - Start both frontend and backend in development mode
- `npm run dev:frontend` - Start only the frontend development server
- `npm run dev:backend` - Start only the backend development server
- `npm run build` - Build the frontend for production
- `npm run preview` - Preview the production build
- `npm run lint` - Run ESLint for code quality

## API Documentation

See [API_DOCUMENTATION.md](./docs/API_DOCUMENTATION.md) for detailed API endpoints and usage.

## Testing Guide

See [TESTING_GUIDE.md](./docs/TESTING_GUIDE.md) for comprehensive testing instructions.

## Configuration

### Environment Variables
The application uses default configurations, but you can customize:

- **Frontend Port**: Default 5173 (Vite default)
- **Backend Port**: Default 3001
- **Upload Directory**: `server/uploads/`
- **File Size Limit**: 5MB per file
- **Max Photos per Review**: 3

### Customization
- Modify `src/config/index.ts` to change API endpoints
- Update `server/index.js` to modify server configuration
- Customize styling in Tailwind CSS classes

## Features in Detail

### Product Management
- Pre-loaded sample products with images from Pexels
- Dynamic rating calculations
- Popular tags extraction
- Sorting options (rating, review count, name)

### Review System
- Star ratings (1-5 stars)
- Text reviews with minimum length validation
- Photo uploads with preview
- Tag system for categorization
- User validation (one review per user per product)

### File Upload
- Image-only uploads
- File size validation
- Multiple file support
- Automatic file naming with timestamps

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support, please open an issue in the GitHub repository or contact the development team.

## Roadmap

- [ ] User authentication system
- [ ] Database integration (PostgreSQL/MongoDB)
- [ ] Advanced search and filtering
- [ ] Review moderation system
- [ ] Email notifications
- [ ] Mobile app development
- [ ] Analytics dashboard

---

Built with ❤️ using React, TypeScript, and Node.js