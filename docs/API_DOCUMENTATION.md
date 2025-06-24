# API Documentation

This document provides detailed information about the ProductReview API endpoints.

## Base URL
```
http://localhost:3001/api
```

## Endpoints

### Products

#### Get All Products
```http
GET /api/products
```

**Description**: Retrieve all products with their ratings and review statistics.

**Response**:
```json
[
  {
    "id": 1,
    "name": "Premium Wireless Headphones",
    "description": "High-quality wireless headphones...",
    "price": 299.99,
    "imageUrl": "https://images.pexels.com/...",
    "averageRating": 4.5,
    "reviewCount": 12,
    "popularTags": ["comfortable", "good sound", "wireless"]
  }
]
```

**Status Codes**:
- `200 OK` - Success

---

### Reviews

#### Get Product Reviews
```http
GET /api/products/:id/reviews
```

**Description**: Retrieve all reviews for a specific product.

**Parameters**:
- `id` (path parameter) - Product ID

**Response**:
```json
[
  {
    "id": 1640995200000,
    "productId": 1,
    "userName": "John Doe",
    "rating": 5,
    "review": "Excellent product! Highly recommended.",
    "tags": ["comfortable", "good sound"],
    "photos": [
      "http://localhost:3001/uploads/photo-123456789.jpg"
    ],
    "createdAt": "2024-01-01T12:00:00.000Z"
  }
]
```

**Status Codes**:
- `200 OK` - Success
- `404 Not Found` - Product not found

#### Create Review
```http
POST /api/reviews
```

**Description**: Create a new review for a product.

**Content-Type**: `multipart/form-data`

**Form Data**:
- `productId` (required) - Product ID
- `userName` (required) - Reviewer's name
- `rating` (optional) - Rating from 1-5
- `review` (optional) - Review text
- `tags` (optional) - Comma-separated tags
- `photos` (optional) - Image files (max 3, 5MB each)

**Validation Rules**:
- Either `rating` or `review` must be provided
- `review` must be at least 10 characters if provided
- Only one review per user per product
- Only image files accepted for photos
- Maximum 3 photos per review
- Maximum 5MB per photo

**Example Request**:
```javascript
const formData = new FormData();
formData.append('productId', '1');
formData.append('userName', 'John Doe');
formData.append('rating', '5');
formData.append('review', 'Great product!');
formData.append('tags', 'comfortable, wireless, good sound');
formData.append('photos', file1);
formData.append('photos', file2);

fetch('/api/reviews', {
  method: 'POST',
  body: formData
});
```

**Response**:
```json
{
  "id": 1640995200000,
  "productId": 1,
  "userName": "John Doe",
  "rating": 5,
  "review": "Great product!",
  "tags": ["comfortable", "wireless", "good sound"],
  "photos": [
    "http://localhost:3001/uploads/photos-1640995200000-123456789.jpg"
  ],
  "createdAt": "2024-01-01T12:00:00.000Z"
}
```

**Status Codes**:
- `201 Created` - Review created successfully
- `400 Bad Request` - Validation error or user already reviewed
- `500 Internal Server Error` - Server error

**Error Response**:
```json
{
  "message": "You have already reviewed this product"
}
```

#### Get Popular Tags
```http
GET /api/reviews/tags
```

**Description**: Retrieve the most popular tags across all reviews.

**Response**:
```json
[
  {
    "tag": "comfortable",
    "count": 15
  },
  {
    "tag": "good sound",
    "count": 12
  },
  {
    "tag": "wireless",
    "count": 10
  }
]
```

**Status Codes**:
- `200 OK` - Success

---

### File Uploads

#### Access Uploaded Files
```http
GET /uploads/:filename
```

**Description**: Access uploaded review photos.

**Parameters**:
- `filename` (path parameter) - File name

**Status Codes**:
- `200 OK` - File found and served
- `404 Not Found` - File not found

---

## Error Handling

### Common Error Responses

#### 400 Bad Request
```json
{
  "message": "Product ID and user name are required"
}
```

#### 500 Internal Server Error
```json
{
  "message": "Internal server error"
}
```

### File Upload Errors

#### File Too Large
```json
{
  "message": "File too large. Maximum size is 5MB."
}
```

#### Invalid File Type
```json
{
  "message": "Only image files are allowed."
}
```

---

## Rate Limiting

Currently, there are no rate limits implemented. In production, consider implementing rate limiting to prevent abuse.

## CORS

The API accepts requests from all origins (`*`). In production, configure CORS to only allow requests from your frontend domain.

## Data Storage

Currently using in-memory storage. Data will be lost when the server restarts. For production use, integrate with a persistent database like PostgreSQL or MongoDB.

## File Storage

Files are stored locally in the `server/uploads/` directory. For production, consider using cloud storage services like AWS S3 or Google Cloud Storage.

## Security Considerations

1. **Input Validation**: All inputs are validated on the server side
2. **File Type Validation**: Only image files are accepted
3. **File Size Limits**: Maximum 5MB per file
4. **Duplicate Prevention**: Users can only review each product once
5. **XSS Prevention**: User inputs should be sanitized before display

## Testing the API

### Using curl

**Get all products**:
```bash
curl http://localhost:3001/api/products
```

**Get product reviews**:
```bash
curl http://localhost:3001/api/products/1/reviews
```

**Create a review**:
```bash
curl -X POST http://localhost:3001/api/reviews \
  -F "productId=1" \
  -F "userName=Test User" \
  -F "rating=5" \
  -F "review=Great product!" \
  -F "tags=test,api"
```

### Using Postman

1. Import the API endpoints into Postman
2. Set the base URL to `http://localhost:3001`
3. For file uploads, use form-data in the Body tab
4. Add the required fields and optional photo files

---

## Changelog

### Version 1.0.0
- Initial API implementation
- Product management endpoints
- Review creation and retrieval
- File upload support
- Tag system implementation