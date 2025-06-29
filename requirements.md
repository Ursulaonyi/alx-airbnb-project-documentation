# 📄 Airbnb Clone Backend - Requirement Specifications

This document outlines the technical and functional specifications for core backend features of the Airbnb Clone project. It includes API endpoints, request/response formats, validation rules, and performance requirements.

---

## 🔐 1. User Authentication

### Overview
Handles user registration, login, and secure session management using JWT tokens.

### Endpoints
- `POST /api/auth/register` – Register a new user
- `POST /api/auth/login` – Login and receive a JWT token

---

### 📌 1.1 Register

- **Endpoint**: `POST /api/auth/register`
- **Request Body**:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "StrongPassword123",
  "role": "guest"
}
```

Validation Rules:
- email: must be in valid format and unique
- password: minimum 8 characters
- role: must be either guest or host

Success Response:
```json
{
  "message": "Registration successful",
  "token": "JWT_TOKEN"
}
```

---

### 📌 1.2 Login

Endpoint: POST /api/auth/login

Request Body:
```json
{
  "email": "john@example.com",
  "password": "StrongPassword123"
}
```

Success Response:
```json
{
  "token": "JWT_TOKEN"
}
```

Validation Rules:
- Valid email/password combo required
- User must exist in the system

⚙️ Performance Criteria
- Token generation < 200ms
- Total request time < 500ms under normal load

---

## 🏘️ 2. Property Management

Overview:
Enables hosts to create, edit, delete, and retrieve property listings.

Endpoints:
- POST /api/properties – Create new property
- GET /api/properties/:id – Get property details
- PUT /api/properties/:id – Update a listing
- DELETE /api/properties/:id – Remove a listing

---

### 📌 2.1 Create Listing

Endpoint: POST /api/properties

Request Body:
```json
{
  "title": "Cozy Apartment in Tokyo",
  "description": "A quiet and well-lit space near downtown.",
  "location": "Tokyo",
  "price": 100,
  "amenities": ["WiFi", "Air Conditioning", "Kitchen"],
  "availableDates": ["2025-07-01", "2025-07-15"]
}
```

Validation Rules:
- title, description, location: required
- price: must be > 0
- availableDates: must be valid ISO date format

Success Response:
```json
{
  "message": "Property created successfully",
  "propertyId": "abc123"
}
```

⚙️ Performance Criteria
- DB write latency < 300ms
- Endpoint response time < 1s

---

## 📅 3. Booking System

Overview:
Allows guests to book available properties and manages reservation conflicts.

Endpoints:
- POST /api/bookings – Create a booking
- GET /api/bookings/:id – View a booking
- DELETE /api/bookings/:id – Cancel a booking

---

### 📌 3.1 Create Booking

Endpoint: POST /api/bookings

Request Body:
```json
{
  "propertyId": "abc123",
  "guestId": "user456",
  "startDate": "2025-07-05",
  "endDate": "2025-07-10"
}
```

Validation Rules:
- Dates must be valid and available
- endDate must be after startDate
- No overlapping bookings for the same property
- User must be authenticated

Success Response:
```json
{
  "message": "Booking confirmed",
  "bookingId": "book789"
}
```

⚙️ Performance Criteria
- Booking conflict check and DB write < 700ms
- Handle 50+ concurrent bookings without conflict
```
