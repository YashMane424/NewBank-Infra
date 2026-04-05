# API Endpoints Reference

## Base URL
```
Development: http://localhost:8080
Frontend Proxy: http://localhost:3000/api (proxies to http://localhost:8080)
```

## Authentication Endpoints

### 1. Login
**Endpoint**: `POST /auth/login`
**Public**: Yes (no authentication required)
**Description**: Authenticate user and receive JWT token

**Request**:
```json
{
  "username": "string",
  "password": "string"
}
```

**Success Response (200)**:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "username": "string",
  "email": "string@example.com",
  "fullName": "string"
}
```

**Error Response (401)**:
```json
{
  "error": "Invalid credentials"
}
```

**cURL Example**:
```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"user123","password":"password123"}'
```

**JavaScript Example**:
```javascript
const response = await fetch('/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    username: 'user123',
    password: 'password123'
  })
});

const data = await response.json();
const token = data.token;

// Store token for future requests
localStorage.setItem('authToken', token);
```

---

### 2. Sign Up / Register
**Endpoint**: `POST /auth/signup`
**Public**: Yes (no authentication required)
**Description**: Register a new user account

**Request**:
```json
{
  "username": "string",
  "email": "string@example.com",
  "password": "string",
  "fullName": "string"
}
```

**Success Response (201)**:
```json
{
  "id": 1,
  "username": "string",
  "email": "string@example.com",
  "fullName": "string",
  "createdAt": "2025-12-31T12:00:00Z"
}
```

**Error Response (400)**:
```json
{
  "error": "User already exists or validation failed"
}
```

**cURL Example**:
```bash
curl -X POST http://localhost:8080/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "username":"newuser",
    "email":"user@example.com",
    "password":"secure123",
    "fullName":"John Doe"
  }'
```

**JavaScript Example**:
```javascript
const response = await fetch('/api/auth/signup', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    username: 'newuser',
    email: 'user@example.com',
    password: 'secure123',
    fullName: 'John Doe'
  })
});

if (response.ok) {
  const data = await response.json();
  console.log('User created:', data);
  // Redirect to login page
}
```

---

## Authenticated Endpoints (Template)

For endpoints that require authentication, include the JWT token in the Authorization header:

```javascript
const token = localStorage.getItem('authToken');

const response = await fetch('/api/your-endpoint', {
  method: 'GET', // or POST, PUT, DELETE
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  }
});
```

---

## Common Headers

### Request Headers
```
Content-Type: application/json
Authorization: Bearer <token>  (for authenticated endpoints)
```

### Response Headers (CORS)
```
Access-Control-Allow-Origin: http://localhost:3000
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS, PATCH
Access-Control-Allow-Headers: *
Access-Control-Expose-Headers: Authorization, Content-Type
```

---

## Error Responses

### 400 - Bad Request
```json
{
  "error": "Invalid request parameters"
}
```

### 401 - Unauthorized
```json
{
  "error": "Missing or invalid authentication token"
}
```

### 403 - Forbidden
```json
{
  "error": "User does not have permission to access this resource"
}
```

### 404 - Not Found
```json
{
  "error": "Resource not found"
}
```

### 500 - Server Error
```json
{
  "error": "Internal server error",
  "message": "Detailed error message"
}
```

---

## Testing with Frontend

### In App.jsx
```javascript
const API_BASE_URL = 'http://localhost:3000/api'; // or '/api' in dev mode

// Example login
const handleLogin = async (username, password) => {
  try {
    const response = await fetch(`${API_BASE_URL}/auth/login`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username, password })
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    
    // Store token
    localStorage.setItem('authToken', data.token);
    
    // Store user info
    localStorage.setItem('user', JSON.stringify({
      username: data.username,
      email: data.email,
      fullName: data.fullName
    }));

    return data;
  } catch (error) {
    console.error('Login failed:', error);
    throw error;
  }
};
```

---

## HTTP Status Codes Reference

| Code | Meaning | When Used |
|------|---------|-----------|
| 200 | OK | Successful GET/POST/PUT |
| 201 | Created | Successful resource creation |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Invalid parameters |
| 401 | Unauthorized | Missing/invalid authentication |
| 403 | Forbidden | Authenticated but no permission |
| 404 | Not Found | Resource doesn't exist |
| 500 | Server Error | Backend error |

---

## Development Tips

### 1. Check CORS is Working
Open DevTools → Network tab, look for:
- `Access-Control-Allow-Origin` header in response
- Should match your frontend origin (http://localhost:3000)

### 2. Debug API Calls
```javascript
const response = await fetch('/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ username: 'user', password: 'pass' })
});

console.log('Status:', response.status);
console.log('Headers:', Object.fromEntries(response.headers));
const data = await response.json();
console.log('Data:', data);
```

### 3. Monitor Backend Logs
Check the backend terminal for request logs:
```
[INFO] POST /auth/login - Username: user123
[DEBUG] Authentication successful
```

---

## Security Notes

⚠️ **Never store sensitive data in localStorage**
- JWT tokens are OK (they're meant to be stateless)
- Passwords should never be stored
- Sensitive user data should be fetched from backend

✅ **Always use HTTPS in production**
✅ **Implement token refresh mechanism**
✅ **Add request timeout handling**
✅ **Validate input on frontend and backend**

---

**Last Updated**: December 31, 2025
