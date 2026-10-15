# TailorPro Atelier — MongoDB Full-Stack

A feature-rich custom-fashion and tailoring management system based on the supplied Row 55 ERD. It includes a polished Next.js frontend, a FastAPI REST API, and a real MongoDB database that can be inspected with MongoDB Compass.

## Stack

- **Frontend:** Next.js 15, React 19, TypeScript, custom responsive atelier UI, Recharts and Lucide icons
- **Backend:** FastAPI, Pydantic validation, PyMongo, REST endpoints and interactive Swagger documentation
- **Database:** MongoDB 7, indexed collections, persistent Docker volume, referential integrity checks in the API
- **Local environment:** Docker Compose

## Quick start

1. Extract the project ZIP.
2. Open Terminal in the extracted `TailorPro-Atelier-MongoDB` folder.
3. Start the app:

   ```bash
   docker compose up --build
   ```

4. Wait for MongoDB, backend and frontend to become healthy, then open:
   - App: http://localhost:3000
   - Database explorer page: http://localhost:3000/database
   - API docs: http://localhost:8000/docs
   - API health: http://localhost:8000/health

The first start may take a few minutes while Docker downloads images and npm installs frontend dependencies. Keep the terminal open while using the app. To stop, press `Ctrl+C`. To start it later, use `docker compose up` from the same folder.

## Connect with MongoDB Compass

MongoDB is published on host port **27018** by default to avoid colliding with a MongoDB server already running locally on the usual port 27017.

1. Open **MongoDB Compass**.
2. Use this connection string:

   ```text
   mongodb://localhost:27018
   ```

3. Connect and expand the **`tailorpro`** database.
4. Inspect each collection and its documents. Data created or edited in the TailorPro frontend is stored here.

If you prefer host port 27017 and it is free, create a `.env` file next to `docker-compose.yml` containing `MONGO_PORT=27017`, then run `docker compose up --build` again. The URI to use in Compass will match that port.

## ERD collections

The application creates and uses the following collections in the `tailorpro` database:

- `customers`
- `measurements`
- `designers`
- `designs`
- `orders`
- `tailors`
- `suppliers`
- `fabrics`
- `garments`
- `garment_fabrics` (the ERD's many-to-many junction entity)
- `trial_sessions`
- `deliveries`

A `counters` collection is used internally to preserve readable sequential IDs for the ERD while each MongoDB document retains its native `_id` ObjectId. References such as `customer_id`, `design_id`, `order_id`, `tailor_id`, and `fabric_id` connect the records across collections.

## Features

- Overview dashboard, order status chart and recent orders
- Customer profiles and body measurements
- Designer directory and design catalogue
- Order tracking and production board
- Tailor assignments, garment records and fabric usage
- Fabric library and suppliers
- Trial scheduling, fitting feedback and delivery tracking
- Database explorer with live collection counts and MongoDB Compass connection details
- Search, create, edit and delete actions across all 12 ERD entities
- Validation of required fields and relationships; readable conflict messages prevent accidental deletion of referenced records
- Demo dataset automatically inserted on first run, with realistic sample customers, styles, fabrics, orders, trials and deliveries
- Persistent MongoDB data volume (`mongodb_data`)

## Useful Docker commands

```bash
# Start all services
 docker compose up --build

# Show service state
 docker compose ps

# Follow backend logs
 docker compose logs -f backend

# Follow all logs
 docker compose logs -f

# Stop services without deleting data
 docker compose down
```

**Do not use `docker compose down -v`** unless you intentionally want to delete the persistent MongoDB database volume.

## API overview

The REST API lives under `/api`; OpenAPI docs are at `/docs`. Useful endpoints include:

- `GET /health`
- `GET /api/dashboard`
- `GET /api/database-summary`
- `GET /api/database/collections/{collection_name}`
- CRUD endpoints for all ERD collections
- `GET /api/production` for a joined production-board view

## Notes

- The MongoDB service is accessible from Compass at the configured host port; the backend uses the private Docker hostname `mongodb` on port 27017.
- If ports 3000, 8000, or the configured MongoDB host port are already in use, free those ports or change the host-side mapping in `docker-compose.yml`.
- The prefilled sign-in screen is a local demo entry screen, not an authentication boundary. This is a local development setup; do not expose the database or API publicly without implementing authentication, authorization, secrets management and backups.
