
# Backend Requirements Specification - Airbnb Clone

## 1. User Authentication

### Objective
Allow users (guests or hosts) to securely register, log in, and manage sessions.

### 🔗 API Endpoints
- `POST /api/register`
- `POST /api/login`
- `GET /api/profile`
- `POST /api/logout`

### Input Specifications
- `POST /api/register`
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securePassword123",
    "role": "guest" // or "host"
  }
  ```

- `POST /api/login`
  ```json
  {
    "email": "john@example.com",
    "password": "securePassword123"
  }
  ```

### Output Specifications
- JWT token for session authentication
- User profile on login

### Validation Rules
- Email must be valid and unique
- Password must be at least 8 characters
- Role must be either `guest` or `host`

### ⚙️ Performance Criteria
- Login response time: ≤ 1s
- Token expiration: 24 hours with refresh token support

---

## 2. Property Management

### Objective
Enable hosts to create, update, and delete property listings.

### API Endpoints
- `POST /api/properties`
- `PUT /api/properties/:id`
- `DELETE /api/properties/:id`
- `GET /api/properties`
- `GET /api/properties/:id`

### Input Specifications
- `POST /api/properties`
  ```json
  {
    "title": "Cozy 2-Bedroom Apartment",
    "location": "Arusha",
    "price": 45,
    "amenities": ["Wi-Fi", "Parking"],
    "available_dates": ["2025-06-01", "2025-06-30"]
  }
  ```

### Output Specifications
- Property object with `id`, `host_id`, and timestamps
- Success message or error details

### Validation Rules
- Title and location are required
- Price must be a positive number
- Dates must be in valid ISO format

### ⚙️ Performance Criteria
- CRUD operations should complete within 2 seconds
- Listings should be cached for faster read access

---

## 3. Booking System

### Objective
Allow guests to book properties and hosts to manage bookings.

### API Endpoints
- `POST /api/bookings`
- `GET /api/bookings/:id`
- `GET /api/bookings?user_id=`
- `DELETE /api/bookings/:id`

### Input Specifications
- `POST /api/bookings`
  ```json
  {
    "property_id": "1234",
    "guest_id": "5678",
    "start_date": "2025-06-10",
    "end_date": "2025-06-15"
  }
  ```

### Output Specifications
- Booking object with `status`, `total_price`, and timestamps
- Success message or availability conflict

### Validation Rules
- Dates must not overlap existing bookings
- End date must be after start date
- Guest cannot book their own listing

### ⚙️ Performance Criteria
- Double-booking prevention with atomic database transaction
- Booking creation latency: ≤ 1.5 seconds

