# Crowdfunding Platform for Startups

A full-stack web application that connects startups with investors. Startups can create and manage funding campaigns, investors can browse and contribute, and admins oversee the entire platform with approval controls.

**GitHub:** https://github.com/Ovaisgithub/crowdfunding-platform

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Java 17, Spring Boot 3.2, Spring Data JPA |
| Auth | Spring Security with JWT (BCrypt password hashing) |
| Database | MySQL 8 |
| Payments | Razorpay Java SDK |
| Frontend | React 19, React Router DOM, Axios, Vite |

---

## Features

### Authentication & Role-Based Access
- Signup and login with JWT-based authentication
- Passwords hashed with BCrypt
- Three roles: **Admin**, **Startup**, **Investor**
- Each role gets a separate dashboard and permissions

### Campaign Management
- Startups can create, edit, and manage campaigns
- Admin approves campaigns before they go live
- Campaign statuses: **Pending → Active → Completed**
- Campaigns organised by category (Technology, Health, Education, etc.)

### Investment & Contributions
- Investors can browse active campaigns and contribute
- Real-time funding progress tracking
- Multi-currency support: INR, USD, EUR
- Secure payments via Razorpay payment gateway

### Subscription System
- Flexible plans: 3, 6, and 12-month options
- Role-specific pricing for Investors and Startups
- Subscription management with Razorpay integration

### Role-Based Access Control

| Action | Admin | Startup | Investor |
|---|---|---|---|
| Approve/reject campaigns | ✅ | ❌ | ❌ |
| Create campaigns | ❌ | ✅ | ❌ |
| Contribute to campaigns | ❌ | ❌ | ✅ |
| View all campaigns | ✅ | ✅ own | ✅ active |
| Manage users | ✅ | ❌ | ❌ |

---

## Running Locally

### Prerequisites
- Java 17+
- Maven 3.6+
- Node.js 18+
- MySQL 8 running locally
- Razorpay account (optional, for payments)

### Backend

```bash
# 1. Create the database
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS crowdfunding_db;"

# 2. Update credentials in:
# backend/crowdfunding-api/src/main/resources/application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/crowdfunding_db
spring.datasource.username=root
spring.datasource.password=your_password

# 3. (Optional) Add Razorpay keys in application.properties
razorpay.key.id=your_key_id
razorpay.key.secret=your_key_secret

# 4. Build and run
cd backend/crowdfunding-api
mvn clean install
mvn spring-boot:run
```

Backend runs at `http://localhost:8080/api`

### Frontend

```bash
cd frontend/crowdfunding-app
npm install
npm run dev
```

Frontend runs at `http://localhost:5173`

---

## API Reference

### Auth
| Method | Endpoint | Access |
|---|---|---|
| POST | `/api/auth/register` | Public |
| POST | `/api/auth/login` | Public |

### Campaigns
| Method | Endpoint | Access |
|---|---|---|
| GET | `/api/campaigns` | All roles |
| GET | `/api/campaigns/:id` | All roles |
| POST | `/api/campaigns` | Startup, Admin |
| PUT | `/api/campaigns/:id` | Startup (own), Admin |
| DELETE | `/api/campaigns/:id` | Admin |
| PUT | `/api/campaigns/:id/approve` | Admin only |

### Other Endpoints
| Endpoint | Description |
|---|---|
| `/api/contributions` | Manage campaign contributions |
| `/api/subscriptions` | Subscription management |
| `/api/categories` | Campaign categories |
| `/api/currencies` | Supported currencies |

---

## Project Structure

```
crowdfunding-platform/
├── backend/
│   └── crowdfunding-api/
│       └── src/main/java/com/crowdfunding/
│           ├── controller/     REST controllers
│           ├── entity/         JPA entities
│           ├── repository/     Data repositories
│           ├── service/        Business logic
│           ├── security/       JWT auth & filters
│           ├── config/         App configuration
│           └── dto/            Data transfer objects
└── frontend/
    └── crowdfunding-app/
        └── src/
            ├── components/     Reusable UI components
            ├── pages/          Page components
            └── services/       Axios API functions
```

---

## Security
- JWT-based stateless authentication
- Role-based access control (RBAC) on all protected routes
- BCrypt password hashing
- CORS configuration for frontend-backend communication
- Input validation and global error handling
- Secure Razorpay payment processing
