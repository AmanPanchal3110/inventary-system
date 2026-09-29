# Inventory & Order Management System

A full-stack inventory and order management application built with **React, FastAPI, PostgreSQL, and Docker**.

The system allows businesses to manage products, customers, and orders while providing real-time stock validation, order lifecycle management, revenue tracking, and an interactive dashboard.

## Live Demo

| Service | URL |
|---|---|
| **Frontend** | https://inventory-system-orpin-eight.vercel.app |
| **Backend API** | https://inventory-system-56zd.onrender.com |
| **Swagger API Docs** | https://inventory-system-56zd.onrender.com/docs |
| **ReDoc API Docs** | https://inventory-system-56zd.onrender.com/redoc |

> The demo is preloaded with sample products and customers for demonstration purposes.

---

## Features

### 📦 Product Management

- Create, update, and delete products
- Product name, SKU, price, and quantity management
- Search products by name or SKU
- Filter products by stock status
- Stock status indicators:
  - In Stock
  - Low Stock
  - Out of Stock
- Paginated product table
- Duplicate SKU validation

### 👥 Customer Management

- Create, update, and delete customers
- Customer name, email, and phone management
- Search by name or email
- Email uniqueness validation
- Customer avatar initials
- Paginated customer table

### 🛒 Order Management

- Create orders with multiple products
- Real-time stock validation
- Automatic stock deduction when an order is created
- Order lifecycle:

```text
Pending
   ├──→ Confirmed
   │
   └──→ Cancelled
```

- Confirmed orders cannot be cancelled
- Cancelling a pending order restores the reserved stock
- Order search and status filtering
- Paginated order listing
- Order detail page
- Sales report export

### 📊 Dashboard

- Total products
- Total customers
- Total orders
- Confirmed revenue
- Low-stock products
- Recent orders
- Animated statistics

### 🎨 UI / UX

- Responsive design
- Dark mode
- Persistent theme selection
- Multiple accent colour themes
- Toast notifications
- Loading states
- Confirmation dialogs
- Smooth animations
- Inline API validation errors

---

# Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 |
| Styling | Tailwind CSS |
| Routing | React Router v6 |
| HTTP Client | Axios |
| Backend | FastAPI |
| ORM | SQLAlchemy |
| Database Driver | asyncpg |
| Validation | Pydantic v2 |
| Database | PostgreSQL 16 |
| Containerization | Docker |
| Local Orchestration | Docker Compose |
| Web Server | nginx |
| Backend Deployment | Render |
| Database Hosting | Render PostgreSQL |
| Frontend Deployment | Vercel |

---

# Architecture

```text
                    ┌──────────────────────┐
                    │        User          │
                    │      Browser         │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       React          │
                    │      Frontend        │
                    │      Vercel          │
                    └──────────┬───────────┘
                               │
                         REST API / HTTP
                               │
                               ▼
                    ┌──────────────────────┐
                    │       FastAPI        │
                    │       Backend        │
                    │       Render         │
                    └──────────┬───────────┘
                               │
                         SQLAlchemy
                               │
                               ▼
                    ┌──────────────────────┐
                    │     PostgreSQL       │
                    │       Database       │
                    │       Render         │
                    └──────────────────────┘
```

---

# Project Structure

```text
inventory-system/
│
├── backend/
│   ├── main.py
│   ├── models.py
│   ├── schemas.py
│   ├── database.py
│   │
│   ├── routers/
│   │   ├── products.py
│   │   ├── customers.py
│   │   └── orders.py
│   │
│   ├── migrations/
│   │   └── init.sql
│   │
│   ├── Dockerfile
│   └── requirements.txt
│
├── frontend/
│   ├── src/
│   │   ├── api/
│   │   │   ├── client.js
│   │   │   └── errors.js
│   │   │
│   │   ├── components/
│   │   │   ├── Badge.jsx
│   │   │   ├── ConfirmDialog.jsx
│   │   │   ├── LoadingSpinner.jsx
│   │   │   ├── Modal.jsx
│   │   │   ├── Pagination.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   └── Toast.jsx
│   │   │
│   │   ├── context/
│   │   │   └── ThemeContext.jsx
│   │   │
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Products.jsx
│   │   │   ├── ProductForm.jsx
│   │   │   ├── Customers.jsx
│   │   │   ├── CustomerForm.jsx
│   │   │   ├── Orders.jsx
│   │   │   ├── OrderDetail.jsx
│   │   │   └── CreateOrder.jsx
│   │   │
│   │   └── index.css
│   │
│   ├── Dockerfile
│   └── nginx.conf
│
├── docker-compose.yml
├── .env
└── README.md
```

---

# Local Development

## Prerequisites

Install:

- Git
- Docker
- Docker Compose

For the Docker-based setup, you do not need to manually install PostgreSQL, Python, or Node.js on your machine.

---

## 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd inventory-system
```

---

## 2. Configure Environment Variables

Create a `.env` file in the project root:

```env
# Database
POSTGRES_DB=inventorydb
POSTGRES_USER=inventoryuser
POSTGRES_PASSWORD=change_me

# Backend
DATABASE_URL=postgresql+asyncpg://inventoryuser:change_me@db:5432/inventorydb
CORS_ORIGINS=http://localhost:3000

# Frontend
REACT_APP_API_URL=http://localhost:8000
```

### Environment Variables

| Variable | Purpose |
|---|---|
| `POSTGRES_DB` | PostgreSQL database name |
| `POSTGRES_USER` | PostgreSQL username |
| `POSTGRES_PASSWORD` | PostgreSQL password |
| `DATABASE_URL` | Backend connection string for PostgreSQL |
| `CORS_ORIGINS` | Frontend origins allowed by the API |
| `REACT_APP_API_URL` | Backend API URL used by React |

> Do not commit `.env` to GitHub. Add it to `.gitignore`.

---

# Run With Docker Compose

Start all services:

```bash
docker compose up --build
```

This starts the application stack including the frontend, backend, and PostgreSQL database.

After the containers start:

### Frontend

```text
http://localhost:3000
```

### Backend

```text
http://localhost:8000
```

### Swagger

```text
http://localhost:8000/docs
```

### ReDoc

```text
http://localhost:8000/redoc
```

To stop the application:

```bash
docker compose down
```

To stop containers and remove their volumes:

```bash
docker compose down -v
```

> Removing volumes deletes the local PostgreSQL data.

---

# Database Initialization

The database schema and sample data are defined in:

```text
backend/migrations/init.sql
```

The SQL file creates the required tables and inserts initial demonstration data.

If the project is configured to initialize the database through Docker Compose, the database setup happens as part of the containerized workflow.

For manual PostgreSQL initialization:

```bash
psql "$DATABASE_URL" -f backend/migrations/init.sql
```

---

# API Reference

## Products

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/products` | Create a product |
| `GET` | `/products` | List products |
| `GET` | `/products/{id}` | Get product details |
| `PUT` | `/products/{id}` | Update a product |
| `DELETE` | `/products/{id}` | Delete a product |

## Customers

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/customers` | Create a customer |
| `GET` | `/customers` | List customers |
| `GET` | `/customers/{id}` | Get customer details |
| `PUT` | `/customers/{id}` | Update a customer |
| `DELETE` | `/customers/{id}` | Delete a customer |

## Orders

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/orders` | Create an order |
| `GET` | `/orders` | List orders |
| `GET` | `/orders/{id}` | Get order details |
| `PATCH` | `/orders/{id}/confirm` | Confirm an order |
| `DELETE` | `/orders/{id}` | Cancel an order |

Interactive API documentation is available through FastAPI Swagger:

```text
/docs
```

---

# Business Logic

The backend enforces the application's core business rules.

### Stock Validation

An order cannot be created if the requested quantity exceeds the available stock.

```text
Requested Quantity > Available Quantity
                ↓
             Reject
```

### Stock Deduction

When an order is successfully created:

```text
Available Stock
       ↓
Stock - Ordered Quantity
```

### Order Cancellation

A pending order can be cancelled.

When cancelled:

```text
Cancelled Order
      ↓
Restore Product Stock
```

### Confirmed Orders

Once an order is confirmed, it cannot be cancelled.

```text
Pending → Confirmed
             ↓
       Cannot cancel
```

### Revenue

The dashboard calculates revenue from **confirmed orders only**.

---

# Error Handling

The application handles common API and validation errors including:

- Invalid request data
- Missing resources
- Duplicate SKU
- Duplicate customer email
- Insufficient stock
- Invalid order state
- Pydantic validation errors

Common HTTP responses include:

```text
200 OK
201 Created
400 Bad Request
404 Not Found
409 Conflict
422 Unprocessable Entity
```

---

# Deployment

The application uses separate services for the frontend, backend, and database.

```text
Frontend → Vercel
Backend  → Render
Database → Render PostgreSQL
```

## Backend Deployment

1. Create a PostgreSQL database on Render.
2. Create a Render Web Service.
3. Connect the GitHub repository.
4. Select Docker as the runtime.
5. Use:

```text
backend/Dockerfile
```

6. Configure the required environment variables.

Example:

```env
DATABASE_URL=<RENDER_POSTGRES_CONNECTION_STRING>
CORS_ORIGINS=https://<YOUR_VERCEL_DOMAIN>
```

## Frontend Deployment

1. Import the GitHub repository into Vercel.
2. Set the root directory to:

```text
frontend
```

3. Configure:

```env
REACT_APP_API_URL=https://<YOUR_RENDER_BACKEND_URL>
```

4. Deploy the application.

After deployment, Vercel can automatically rebuild the frontend when changes are pushed to the configured branch.

---

# Security Notes

Before deploying to production:

- Use a strong PostgreSQL password.
- Never commit `.env` files.
- Store production secrets in Render/Vercel environment variables.
- Restrict `CORS_ORIGINS` to trusted frontend domains.
- Do not expose database credentials in frontend code.
- Use HTTPS for production services.

---

# Future Improvements

Possible extensions include:

- User authentication and role-based access control
- JWT authentication
- Product categories
- Supplier management
- Purchase orders
- Inventory transaction history
- Advanced analytics
- Email notifications
- Audit logs
- Redis caching
- Automated testing
- CI/CD pipeline
- Kubernetes deployment
- Database migrations with Alembic

---

# Author

**Aman Panchal**

Full-Stack / Backend Developer

Technologies: **Python · FastAPI · React · PostgreSQL · Docker · REST APIs**

---

## License

This project is intended for educational, portfolio, and demonstration purposes.