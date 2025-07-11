# Technical & Functional Requirements – Airbnb Clone Backend

This document outlines the backend requirements for the following core features:
1. User Authentication
2. Property Management
3. Booking System

---

## 1. User Authentication

### 🔧 Functional Requirements
- Users (Guests, Hosts, Admins) must be able to register and log in.
- Authenticated users can access personalized features based on their role.

### 🌐 API Endpoints
| Method | Endpoint         | Description            |
|--------|------------------|------------------------|
| POST   | /api/auth/register | Register a new user   |
| POST   | /api/auth/login    | Authenticate and log in |
| GET    | /api/auth/profile  | Get user profile      |

### 🧾 Input / Output
- **Register (POST /register)**
  - Input: `{ "email": "user@example.com", "password": "secure", "role": "guest" }`
  - Output: `{ "message": "User registered successfully", "token": "<JWT>" }`

- **Login (POST /login)**
  - Input: `{ "email": "user@example.com", "password": "secure" }`
  - Output: `{ "token": "<JWT>", "user": { id, name, role } }`

### 🛡️ Validation Rules
- Email must be unique and valid format.
- Password minimum 8 characters.
- Role must be one of: `guest`, `host`, `admin`.

### 🚀 Performance Criteria
- Response time < 200ms for login/register under normal load.
- Scalable authentication system with JWT-based session.

---

## 2. Property Management

### 🔧 Functional Requirements
- Hosts can create, update, and delete their property listings.
- Guests can view and search properties.

### 🌐 API Endpoints
| Method | Endpoint              | Description               |
|--------|-----------------------|---------------------------|
| POST   | /api/properties       | Create a new listing      |
| GET    | /api/properties       | Get all properties        |
| GET    | /api/properties/:id   | Get property details      |
| PUT    | /api/properties/:id   | Update a listing          |
| DELETE | /api/properties/:id   | Delete a property         |

### 🧾 Input / Output
- **Create Listing**
  - Input: `{ "title": "Beach House", "pricePerNight": 100, "location": "Miami" }`
  - Output: `{ "message": "Property created", "property_id": "uuid" }`

- **Get All Properties**
  - Output: `[ { "id": "uuid", "title": "...", "pricePerNight": ... }, ... ]`

### 🛡️ Validation Rules
- Price must be a positive number.
- Title and location are required fields.
- Only authenticated hosts can create/update/delete.

### 🚀 Performance Criteria
- Indexed search by location and price.
- Max response time for listings: < 300ms.

---

## 3. Booking System

### 🔧 Functional Requirements
- Guests can book available properties.
- Hosts can view bookings for their properties.
- Bookings include total price, dates, and status.

### 🌐 API Endpoints
| Method | Endpoint              | Description                |
|--------|-----------------------|----------------------------|
| POST   | /api/bookings         | Create a booking           |
| GET    | /api/bookings/my      | View guest booking history |
| GET    | /api/bookings/host    | View bookings as host      |
| PUT    | /api/bookings/:id     | Cancel a booking           |

### 🧾 Input / Output
- **Book a Property**
  - Input: `{ "propertyId": "uuid", "startDate": "2025-07-10", "endDate": "2025-07-15" }`
  - Output: `{ "message": "Booking confirmed", "bookingId": "uuid", "totalPrice": 500 }`

- **Cancel Booking**
  - Output: `{ "message": "Booking cancelled", "refundStatus": "initiated" }`

### 🛡️ Validation Rules
- Booking dates must not overlap existing bookings.
- Total price = pricePerNight × number of nights.
- Only guests can book or cancel; hosts can only view.

### 🚀 Performance Criteria
- Fast availability checks (< 150ms).
- Booking transactions are atomic and consistent.

---
