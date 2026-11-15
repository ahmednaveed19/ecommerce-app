# E-Commerce Web Application

E-Commerce Web Application is a full-stack online store demo built to run locally. Shoppers can browse and filter a product catalog, manage a basket, create an account, and place orders with a Stripe-powered checkout. The API also supports order history and saved shipping addresses.

## Features

- Product catalog with search, filters, sorting, and pagination
- Shopping basket with optimistic client-side updates
- Registration and sign-in, user addresses, and authenticated order history
- Checkout with Stripe Payment Intents and webhook-based payment status updates
- Responsive storefront with both light and dark mode
- SQLite database initialized with Entity Framework Core migrations and sample products

## Technology stack

| Area | Technologies |
| --- | --- |
| Frontend | React 19, TypeScript, Vite, React Router |
| UI components | Material UI (MUI) |
| State and API data | Redux Toolkit and React-Redux |
| Backend | ASP.NET Core Web API on .NET 10 |
| Data access and database | Entity Framework Core 10, SQLite |
| Authentication | ASP.NET Core Identity with cookie-based authentication |
| Payments | Stripe Elements on the client and Stripe Payment Intents in the API |

### Redux, RTK Query, Entity Framework, Identity, and Stripe

- **Redux Toolkit** provides the frontend's shared store for UI state and feature data.
- **RTK Query** defines the API endpoints for the catalog, account, basket, checkout, and orders, and manages request state and caching.
- **Entity Framework Core** maps the API's products, baskets, orders, and Identity data to SQLite. Migrations are applied when the API starts.
- **ASP.NET Core Identity** handles user registration, login, roles, and authentication cookies.
- **Stripe** powers card checkout. The frontend uses Stripe Elements, while the API creates Payment Intents and processes payment webhooks.

## Quick start

### Prerequisites

- .NET 10 SDK
- Node.js and npm
- Stripe test-mode API keys to exercise checkout

### 1. Configure and run the API

Before running the API, update the `StripeSettings` section in `API/appsettings.Development.json` with your Stripe publishable key, secret key, and webhook signing secret. Do not commit real Stripe keys to source control.

The API uses HTTPS at `https://localhost:5001`; Swagger is available at `https://localhost:5001/swagger` in Development. On startup, it applies database migrations and seeds sample products.

```bash
cd API
dotnet restore
dotnet run
```

### 2. Configure and run the frontend

In another terminal, from the repository root:

```bash
cd client
npm install
```

Create `client/.env` with your Stripe **publishable** test key:

```dotenv
VITE_STRIPE_PK=pk_test_your_publishable_key
```

Then start the development server:

```bash
npm run dev
```

Open `https://localhost:3000`. The frontend is configured to call the API at `https://localhost:5001/api`. The Vite development server uses a local HTTPS certificate; trust its certificate if prompted.

For local payment-status updates, run the Stripe CLI in a third terminal and configure the webhook signing secret it prints:

```bash
stripe listen -f https://localhost:5001/api/payments/webhook -e payment_intent.succeeded,payment_intent.payment_failed
```
