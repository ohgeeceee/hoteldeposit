# OpenDeposit (hoteldeposit)

**Open source hotel deposit & policy platform.** Search hotels, brands, and cities to see deposit amounts, hold policies, and incidentals before you book — with an admin API for importing and curating the data.

## Stack

- **Backend:** Node.js (≥18) + Express 4
- **Database:** PostgreSQL (`pg`) — a MariaDB/MySQL schema variant is included (`schema-mysql.sql`)
- **Frontend:** Static HTML/CSS/JS served from `public/`
- **Data ingest:** `scraper.py` (Python) + `import.js` (Node)
- **Deploy:** Render via `render.yaml`; helper scripts `deploy.sh` / `deploy_auto.py`

## Quick start

```bash
npm install
cp .env.example .env   # fill in your database credentials
psql -d hoteldeposit -f schema.sql
npm start              # serves on http://localhost:3000
```

### Environment variables

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string (or use `DB_HOST`/`DB_USER`/`DB_PASSWORD`/`DB_NAME` for MySQL variant) |
| `PORT` | Server port (default `3000`) |
| `NODE_ENV` | `production` enables SSL for the DB connection |

## API overview

**Public**

- `GET /api/search` — search hotels by name/city/brand
- `GET /api/hotels` / `GET /api/hotel/:slug` — hotel listings and detail
- `GET /api/brands` / `GET /api/brands/:brandName/hotels` — browse by brand
- `GET /api/cities` — browse by city
- `GET /api/policies` — deposit/hold policies

**Admin**

- `POST /api/admin/import` — bulk import scraped data
- `POST /api/admin/hotel` · `PUT /api/admin/hotel/:id` · `DELETE /api/admin/hotel/:id`
- `PUT /api/admin/policy/:hotelId` — update a hotel's policy
- `POST /api/admin/brand` · `DELETE /api/admin/brand/:id`
- `GET /api/admin/analytics` — dataset stats

## Importing data

1. Run `scraper.py` to collect raw hotel policy data.
2. Transform/load with `node import.js`, or push JSON directly to `POST /api/admin/import`.

## Deployment

The repo ships a `render.yaml` for one-click Render deployment. Set the environment variables above in the Render dashboard, then run `deploy.sh` (or `deploy_auto.py`) for subsequent deploys.
