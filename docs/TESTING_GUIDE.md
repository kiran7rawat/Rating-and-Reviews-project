# Testing Guide

This guide provides comprehensive instructions for testing the ProductReview application.

## Table of Contents
1. [Setup for Testing](#setup-for-testing)
2. [Manual Testing](#manual-testing)
3. [API Testing](#api-testing)
4. [Frontend Testing](#frontend-testing)
5. [File Upload Testing](#file-upload-testing)
6. [Error Handling Testing](#error-handling-testing)
7. [Performance Testing](#performance-testing)
8. [Browser Compatibility Testing](#browser-compatibility-testing)

## Setup for Testing

### Prerequisites
1. Ensure the application is running:
   ```bash
   npm run dev
   ```
2. Frontend should be accessible at `http://localhost:5173`
3. Backend should be running on `http://localhost:3001`

### Test Data
The application comes with 6 pre-loaded products for testing purposes.

## Manual Testing

### 1. Product Display Testing

**Test Case**: Verify product catalog displays correctly
- **Steps**:
  1. Open `http://localhost:5173`
  2. Verify 6 products are displayed
  3. Check that each product shows:
     - Product image
     - Product name
     - Description
     - Price
     - Rating (initially 0 stars)
     - Review count (initially 0)

**Expected Result**: All products display with correct information and layout is responsive.

### 2. Sorting Functionality Testing

**Test Case**: Verify sorting options work correctly
- **Steps**:
  1. Use the sort dropdown in the header
  2. Test each sorting option:
     - Highest Rated
     - Most Reviews
     - Name A-Z
  3. Verify products reorder correctly

**Expected Result**: Products sort according to selected criteria.

### 3. Review Creation Testing

**Test Case**: Create a complete review
- **Steps**:
  1. Click "Write Review" on any product
  2. Fill in the review form:
     - Name: "Test User"
     - Rating: 5 stars
     - Review: "This is an excellent product with great quality and value."
     - Tags: "excellent, quality, value"
  3. Upload 1-2 test images (use any image files < 5MB)
  4. Click "Submit Review"

**Expected Result**: 
- Review submits successfully
- Modal closes
- Product card updates with new rating and review count
- Review appears when clicking "Show Reviews"

### 4. Review Display Testing

**Test Case**: Verify reviews display correctly
- **Steps**:
  1. After creating a review, click "Show Reviews" on the product
  2. Verify the review displays with:
     - User name
     - Date
     - Star rating
     - Review text
     - Tags
     - Uploaded photos

**Expected Result**: All review information displays correctly with proper formatting.

## API Testing

### Using Browser Developer Tools

1. **Open Developer Tools** (F12)
2. **Go to Network tab**
3. **Perform actions** and observe API calls

### Test API Endpoints

#### 1. Get Products
```javascript
// In browser console
fetch('http://localhost:3001/api/products')
  .then(response => response.json())
  .then(data => console.log(data));
```

#### 2. Get Product Reviews
```javascript
// In browser console
fetch('http://localhost:3001/api/products/1/reviews')
  .then(response => response.json())
  .then(data => console.log(data));
```

#### 3. Create Review (without files)
```javascript
// In browser console
const formData = new FormData();
formData.append('productId', '1');
formData.append('userName', 'API Test User');
formData.append('rating', '4');
formData.append('review', 'Testing via API - this product works well');
formData.append('tags', 'api-test, functional');

fetch('http://localhost:3001/api/reviews', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(data => console.log(data));
```

### Using curl Commands

#### Get all products:
```bash
curl -X GET http://localhost:3001/api/products
```

#### Get reviews for product 1:
```bash
curl -X GET http://localhost:3001/api/products/1/reviews
```

#### Create a review:
```bash
curl -X POST http://localhost:3001/api/reviews \
  -F "productId=1" \
  -F "userName=Curl Test User" \
  -F "rating=5" \
  -F "review=Testing with curl command" \
  -F "tags=curl,test,command"
```

## Frontend Testing

### 1. Responsive Design Testing

**Test different screen sizes**:
- Desktop (1920x1080)
- Tablet (768x1024)
- Mobile (375x667)

**Steps**:
1. Open browser developer tools
2. Toggle device toolbar
3. Test different device presets
4. Verify layout adapts correctly

### 2. Interactive Elements Testing

**Test all interactive elements**:
- Buttons (hover states, click feedback)
- Form inputs (focus states, validation)
- Star ratings (hover and click)
- Modal open/close
- Photo upload interface

### 3. Loading States Testing

**Test loading indicators**:
1. Refresh the page and observe initial loading
2. Submit a review and observe submission loading
3. Click "Show Reviews" and observe review loading

## File Upload Testing

### 1. Valid File Upload Testing

**Test Case**: Upload valid image files
- **Steps**:
  1. Open review modal
  2. Click upload area
  3. Select 1-3 image files (JPG, PNG, GIF)
  4. Verify preview appears
  5. Submit review

**Expected Result**: Files upload successfully and appear in review.

### 2. File Validation Testing

**Test Case**: Test file size limits
- **Steps**:
  1. Try uploading a file > 5MB
  2. Verify error message appears

**Test Case**: Test file type validation
- **Steps**:
  1. Try uploading non-image files (PDF, TXT, etc.)
  2. Verify error message appears

**Test Case**: Test maximum file count
- **Steps**:
  1. Try uploading more than 3 files
  2. Verify only first 3 are accepted

### 3. File Removal Testing

**Test Case**: Remove uploaded files
- **Steps**:
  1. Upload multiple files
  2. Click X button on file previews
  3. Verify files are removed

## Error Handling Testing

### 1. Form Validation Testing

**Test empty form submission**:
- Submit form without name
- Submit form without rating or review
- Submit form with review < 10 characters

**Expected Results**: Appropriate error messages display.

### 2. Duplicate Review Testing

**Test Case**: Attempt duplicate review
- **Steps**:
  1. Submit a review for a product
  2. Try to submit another review for the same product with the same name
  3. Verify error message appears

### 3. Network Error Testing

**Test Case**: Simulate network issues
- **Steps**:
  1. Open developer tools
  2. Go to Network tab
  3. Set throttling to "Offline"
  4. Try to submit a review
  5. Verify error handling

## Performance Testing

### 1. Load Time Testing

**Measure page load times**:
1. Open developer tools
2. Go to Network tab
3. Refresh page
4. Check load times for:
   - Initial page load
   - Image loading
   - API responses

### 2. Large File Upload Testing

**Test with maximum file sizes**:
1. Upload files close to 5MB limit
2. Monitor upload progress
3. Verify successful upload

### 3. Multiple Reviews Testing

**Test with many reviews**:
1. Create 10+ reviews for a single product
2. Test "Show Reviews" performance
3. Verify scrolling works smoothly

## Browser Compatibility Testing

### Test in Multiple Browsers

**Required browsers**:
- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

**Test features in each browser**:
- Basic functionality
- File uploads
- CSS animations
- Form validation
- Modal interactions

## Automated Testing Checklist

### Daily Testing Checklist

- [ ] Application starts without errors
- [ ] All products display correctly
- [ ] Review creation works
- [ ] File upload functions
- [ ] API endpoints respond correctly
- [ ] No console errors
- [ ] Responsive design works

### Pre-deployment Testing Checklist

- [ ] All manual tests pass
- [ ] API tests pass
- [ ] File upload tests pass
- [ ] Error handling works
- [ ] Performance is acceptable
- [ ] Cross-browser compatibility confirmed
- [ ] No security vulnerabilities
- [ ] All features work as expected

## Common Issues and Solutions

### Issue: Reviews not displaying
**Solution**: Check browser console for API errors, verify backend is running.

### Issue: File upload fails
**Solution**: Check file size and type, verify uploads directory exists.

### Issue: Styling issues
**Solution**: Clear browser cache, check Tailwind CSS is loading.

### Issue: API connection errors
**Solution**: Verify backend is running on port 3001, check CORS settings.

## Test Data Cleanup

After testing, you may want to reset the application:

1. **Restart the server** to clear in-memory data
2. **Delete uploaded files** from `server/uploads/` directory
3. **Clear browser cache** for fresh start

## Reporting Issues

When reporting issues, include:
1. Steps to reproduce
2. Expected vs actual behavior
3. Browser and version
4. Console error messages
5. Screenshots if applicable

---

## Test Results Template

Use this template to document test results:

```
Test Date: [Date]
Tester: [Name]
Browser: [Browser and Version]
Test Environment: [Local/Staging/Production]

Test Results:
✅ Product display
✅ Review creation
✅ File upload
❌ Issue found: [Description]

Notes: [Additional observations]
```