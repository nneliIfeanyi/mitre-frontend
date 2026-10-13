# Deployment Checklist

## Local: separate frontend and API

- API: XAMPP Apache serves `api/` at `http://localhost/mitre2/api/`.
- Frontend: run a static server with `frontend/` as its document root (for example, VS Code Live Server on port 5500).
- In the project-root `.env`, set `APP_URL=http://localhost/mitre2/api` and set `FRONTEND_URL` to the exact frontend origin, such as `http://127.0.0.1:5500,http://localhost:5500`.
- The local frontend API URL is set in `public/assets/js/config.js` (path relative to `frontend/`).

## Production: separate hosts

### 1. Database

Create the database and import the schema:

```sql
CREATE DATABASE training_school CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

```powershell
mysql -u USER -p training_school < api/sql/schema.sql
```

### 2. API

Point the API host's document root (for example, `https://api.yourdomain.com`) at the `api/` folder. Configure the project-root `.env`:

```dotenv
APP_DISPLAY_ERRORS=0
APP_URL=https://api.yourdomain.com
FRONTEND_URL=https://yourdomain.com
DB_HOST=localhost
DB_NAME=your_database
DB_USER=your_database_user
DB_PASS=your_database_password
SMS_ENABLED=false
```

Make `api/uploads/` writable by the web server.

### 3. Frontend

Point the frontend host's document root (for example, `https://yourdomain.com`) at the `frontend/` folder. Update `public/assets/js/config.js` (relative to `frontend/`) to use the API URL:

```js
window.API_BASE = 'https://api.yourdomain.com';
```

### 4. Security

- [ ] Change the default admin password.
- [ ] Enable HTTPS on both hosts.
- [ ] Set `APP_DISPLAY_ERRORS=0`.
- [ ] Set `FRONTEND_URL` to the real frontend origin.
- [ ] Keep `.env` out of the public document root and source control.
- [ ] Back up the database.
