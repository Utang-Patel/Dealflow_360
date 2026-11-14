# DealFlow360

**B2B Deal Management, CPQ & Revenue Orchestration Platform**

DealFlow360 is an enterprise B2B SaaS platform that covers the full deal lifecycle: building quotations (CPQ), pricing and approvals, multi-warehouse fulfillment, subscriptions, billing, and a customer negotiation portal. It is built as a **Django REST API** backend with a **React** frontend.

> Built as part of the Odoo Hackathon 2026.

---

## Features

- **CPQ (Configure, Price, Quote):** customers, customer tiers, product catalog with variants, quotations and line items.
- **Automatic deal scoring:** gross, discount, net, cost, margin percentage and a blended risk score are calculated for every quotation.
- **Approvals:** approval requests for quotations that need review, plus discount tier rules and recommendation rules.
- **Deal health alerts:** risk alerts raised against deals.
- **Inventory and fulfillment:** multiple warehouses, stock levels, automatic and manual fulfillment, split sourcing and backorder consolidation.
- **Subscriptions:** plans, pause, resume, modify and cancel, proration, recurring billing, and a command to process due subscriptions.
- **Billing:** invoices, payments and credit notes.
- **Customer portal:** negotiation requests with sales comments and review.
- **Authentication and RBAC:** email login with JWT (access and refresh tokens, logout with token blacklist) and roles: admin, finance manager, sales manager, sales representative and customer.
- **API documentation:** Swagger UI and ReDoc generated with drf-spectacular.
- **Django Admin:** customised admin dashboard with autocomplete search for related records.

## Tech Stack

| Layer | Technologies |
| --- | --- |
| Backend | Python, Django, Django REST Framework |
| Auth | JWT (`djangorestframework-simplejwt`), session auth, custom user model |
| API docs | `drf-spectacular` (Swagger UI, ReDoc, OpenAPI schema) |
| Database | MySQL (via PyMySQL), SQLite fallback for local development |
| Frontend | React 18, Vite, Tailwind CSS, React Router, Recharts, lucide-react |
| Tooling | Git, GitHub, oxlint, python-dotenv, django-cors-headers |

## Project Structure

```
DealFlow360/
├── backend/
│   ├── dealflow360/        # Django project (settings, urls)
│   ├── authentication/     # Custom User, Role, JWT login/register/logout
│   ├── deals/              # Customers, products, quotations, approvals, deal health
│   ├── inventory/          # Warehouses, stock, fulfillment, backorders
│   ├── billing/            # Invoices, payments, credit notes
│   ├── subscriptions/      # Plans, lifecycle, proration, recurring billing
│   ├── portal/             # Customer negotiation portal
│   ├── manage.py
│   └── requirements.txt
└── frontend/
    ├── src/
    │   ├── components/     # UI components and layout (Sidebar, Header)
    │   ├── pages/          # Dashboard, quotations, approvals, fulfillment, etc.
    │   ├── context/        # Auth, data and toast contexts
    │   └── data/           # Mock dataset (INR)
    ├── package.json
    └── vite.config.js
```

## Getting Started

### Prerequisites

- Python 3.11 or newer
- Node.js 18 or newer and npm
- MySQL 8 (optional, SQLite is used if no database is configured)

### 1. Backend setup

```bash
cd backend

# Create and activate a virtual environment
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Create your environment file
cp .env.example .env        # Windows: copy .env.example .env
```

Edit `.env` with your own values (see [Environment Variables](#environment-variables)), then:

```bash
# Create the database tables
python manage.py migrate

# Seed roles, users and sample data
python manage.py seed_auth_users
python manage.py seed_20_records

# Start the API server
python manage.py runserver
```

The API runs at `http://127.0.0.1:8000/`.

> The seed commands create demo accounts. Change their passwords before using this anywhere other than your own machine.

### 2. Frontend setup

```bash
cd frontend
npm install
npm run dev
```

The app runs at `http://localhost:5173/`.

> The frontend currently uses mock data from `src/data/mockData.js` and is not yet connected to the backend API.

## Environment Variables

Create `backend/.env`:

| Variable | Description | Example |
| --- | --- | --- |
| `DEBUG` | Django debug mode | `True` |
| `SECRET_KEY` | Django secret key | a long random string |
| `ALLOWED_HOSTS` | Allowed hosts, comma separated | `localhost,127.0.0.1` |
| `DB_ENGINE` | `mysql` or `sqlite` | `mysql` |
| `DB_NAME` | Database name | `dealflow` |
| `DB_USER` | Database user | `root` |
| `DB_PASSWORD` | Database password | your password |
| `DB_HOST` | Database host | `127.0.0.1` |
| `DB_PORT` | Database port | `3306` |

If `DB_NAME` is not set, the project falls back to SQLite (`db.sqlite3`). Never commit your real `.env` file.

## API Overview

| Area | Base path |
| --- | --- |
| Authentication | `/api/auth/` (`register`, `login`, `token/refresh`, `logout`, `me`) |
| Deals and CPQ | `/api/` (`customers`, `customer-tiers`, `products`, `variants`, `categories`, `quotations`, `quotation-items`, `approval-requests`, `discount-tier-rules`, `recommendation-rules`, `deal-health-alerts`) |
| Inventory | `/api/` (`warehouses`, `inventory`, `fulfillment-splits`, `fulfillment/auto`, `fulfillment/manual`, `fulfillment/backorders/consolidate`) |
| Subscriptions | `/api/subscriptions/` |
| Billing | `/api/billing/` |
| Portal | `/api/portal/` |

Interactive documentation (when the server is running):

- Swagger UI: `http://127.0.0.1:8000/docs/`
- ReDoc: `http://127.0.0.1:8000/redoc/`
- OpenAPI schema: `http://127.0.0.1:8000/api/schema/`
- Django Admin: `http://127.0.0.1:8000/admin/`

Protected endpoints need a JWT header: `Authorization: Bearer <access_token>`.

## Useful Commands

```bash
# Run backend tests
python manage.py test

# Process subscriptions that are due for billing
python manage.py process_due_subscriptions

# Build the frontend for production
cd frontend && npm run build
```

## Roadmap

- Connect the React frontend to the Django REST API
- Add tests for inventory, billing, subscriptions and portal modules
- Add GST-aware invoicing to the billing module
- Docker setup for one-command local start

## Author

**Utang Patel**
GitHub: [@Utang-Patel](https://github.com/Utang-Patel)
