# MITRE Frontend

The frontend and PHP API are independent: run or deploy the `frontend/` and `api/` directories separately. The frontend is static HTML/JS; the API requires PHP and MySQL.

## Run locally with XAMPP

1. Start Apache and MySQL in XAMPP. With this project at `C:\xampp\htdocs\mitre2`, the API is available at `http://localhost/mitre2/api/`.
2. Start a static server with its document root set to this `frontend/` directory. In VS Code, open `frontend/index.html` and choose **Open with Live Server**. The frontend will usually be at `http://127.0.0.1:5500/`.
3. Ensure the project-root `.env` contains:
   ```dotenv
   APP_URL=http://localhost/mitre2/api
   FRONTEND_URL=http://localhost:5500,http://127.0.0.1:5500
   ```
   The API's CORS policy uses these exact frontend origins.

The frontend API setting is in `public/assets/js/config.js` relative to this folder. Its local default points to the XAMPP API at `http://localhost/mitre2/api`. Check `http://localhost/mitre2/api/health.php` to confirm the API is responding. The local database schema is `../api/sql/schema.sql`.

## Deploy separately

Set the frontend host's document root to `frontend/`. Set the API host's document root to `api/`, configure the project-root `.env` with database credentials and the frontend origin, and update `public/assets/js/config.js` to the API's public URL. See `deploy.md` for the deployment checklist.
