# Digital Mandi Price Information Systems

## Overview

A full-stack agriculture platform for Indian farmers built with React + Vite frontend, Express.js backend, and PostgreSQL database.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **Frontend**: React + Vite + Tailwind CSS + ShadcnUI
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **State**: Zustand
- **Forms**: React Hook Form + Hookform Resolvers

## Features

1. **Real-Time Mandi Prices** — Fetches from data.gov.in API (fallback data included)
2. **Weather Information** — City/village weather via OpenWeatherMap API (fallback data included)
3. **Agriculture Store** — 15 products across seeds, fertilizers, pesticides, tools, irrigation with cart
4. **Crop Selling Marketplace** — Create and browse crop listings with filters
5. **Government Schemes** — 6 major schemes + news updates
6. **Multi-Language** — English, Hindi, Gujarati switcher
7. **Farmer Auth** — Register/login with session management
8. **Notifications** — Price alerts, scheme updates, marketplace offers
9. **Order Management** — Cart to checkout flow

## Structure

```text
artifacts/
├── api-server/         # Express API server
│   └── src/routes/     # mandi, weather, store, cart, marketplace, schemes, auth, notifications, orders
└── digital-mandi/      # React + Vite frontend
    └── src/
        ├── pages/      # home, mandi-prices, weather, store, sell-crops, schemes, auth, profile
        ├── components/ # layout (header, cart drawer)
        └── lib/        # store.ts (zustand), translations.ts

lib/
├── api-spec/           # OpenAPI spec + Orval codegen config
├── api-client-react/   # Generated React Query hooks
├── api-zod/            # Generated Zod schemas from OpenAPI
└── db/                 # Drizzle ORM schema + DB connection
    └── src/schema/     # farmers, products, cart, listings, orders, notifications
```

## Environment Variables

- `DATABASE_URL` — PostgreSQL connection string (auto-provisioned by Replit)
- `DATA_GOV_API_KEY` — Optional: data.gov.in API key for live mandi prices
- `OPENWEATHER_API_KEY` — Optional: OpenWeatherMap API key for live weather

## API Routes

All routes under `/api`:
- `GET /mandi/prices` — Mandi prices (with state/commodity/market filters)
- `GET /mandi/states` — List of states
- `GET /mandi/commodities` — List of commodities
- `GET /weather?city=` — Weather by city
- `GET /store/products` — Agriculture store products
- `GET|POST /cart` — Cart management
- `DELETE /cart/:productId` — Remove cart item
- `GET|POST /marketplace/listings` — Crop listings
- `GET /schemes` — Government schemes
- `POST /auth/register` — Farmer registration
- `POST /auth/login` — Farmer login
- `POST /auth/logout` — Logout
- `GET /auth/me` — Current farmer profile
- `GET /notifications` — Notifications
- `GET|POST /orders` — Order history and creation

## Development

```bash
# Start API server
pnpm --filter @workspace/api-server run dev

# Start frontend
pnpm --filter @workspace/digital-mandi run dev

# Run codegen after API spec changes
pnpm --filter @workspace/api-spec run codegen

# Push DB schema changes
pnpm --filter @workspace/db run push
```
