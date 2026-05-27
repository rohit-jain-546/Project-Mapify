# Project - Mapify

Mapify is a Flask web application that combines user authentication with Mapbox-powered route visualization. Users can sign up, log in, enter a start and destination location, and view a driving route on an interactive map.

## Features

- User registration and login with encrypted passwords (`Flask-Bcrypt`)
- Session-based authentication and protected routes (`Flask-Login`)
- Route plotting between two locations using Mapbox APIs
- Interactive map rendering with `mapbox-gl-js`
- Persistent user storage with `Flask-SQLAlchemy`

## Tech Stack

- Python
- Flask
- Flask-SQLAlchemy
- Flask-Login
- Flask-WTF / WTForms
- Flask-Bcrypt
- MySQL (via `PyMySQL`) or any SQL database configured through `DATABASE_URL`
- Mapbox GL JS + Geocoding + Directions APIs
- Gunicorn (production server)

## Project Structure

```text
Mapify/
|-- main.py
|-- requirements.txt
|-- nixpacks.toml
|-- static/
|   |-- css/
|   `-- images/
`-- templates/
    |-- index.html
    |-- login.html
    |-- signup.html
    `-- home.html
```

## Prerequisites

- Python 3.9+
- A Mapbox account and access token
- A database instance (for users table)

## Environment Variables

Create a `.env` file (or set variables in your deployment platform):

```env
DATABASE_URL=mysql+pymysql://username:password@host:3306/database_name
SECRET_KEY=your_strong_secret_key
MAPBOX_ACCESS_TOKEN=your_mapbox_access_token
PORT=5000
```

Notes:
- `DATABASE_URL` is required.
- `SECRET_KEY` should be set explicitly in production.
- `MAPBOX_ACCESS_TOKEN` is used by the app for map functionality.

## Installation

```bash
# 1) Clone repository
git clone <your-repo-url>
cd Mapify

# 2) Create virtual environment
python -m venv .venv

# 3) Activate virtual environment
# Windows (PowerShell)
.\.venv\Scripts\Activate.ps1
# macOS/Linux
source .venv/bin/activate

# 4) Install dependencies
pip install -r requirements.txt
```

## Database Setup

Open a Python shell in the project root and run:

```python
from main import app, db
with app.app_context():
    db.create_all()
```

This creates the `user` table defined in `main.py`.

## Run Locally

```bash
python main.py
```

App runs on:
- `http://127.0.0.1:5000`
- or `http://0.0.0.0:5000`

## Production Run

Gunicorn command (already configured in `nixpacks.toml`):

```bash
gunicorn main:app
```

## Application Flow

1. `GET /` shows landing page.
2. `GET/POST /signup` creates a new user account.
3. `GET/POST /login` authenticates user.
4. `GET/POST /home` (login required) accepts start/end locations and renders route.
5. `GET /logout` logs out the user.

## Security Notes

- Passwords are hashed before storing.
- Keep `SECRET_KEY` private.
- Do not hardcode API tokens in templates or JavaScript for production.
- Restrict and rotate your Mapbox token periodically.

## Deployment

This project includes `nixpacks.toml`:

```toml
[build]
builder = "nixpacks"

[start]
cmd = "gunicorn main:app"
```

It can be deployed to platforms that support Nixpacks/Gunicorn (for example: Railway).

## Future Improvements

- Flash messages for login/signup errors
- Better form validation feedback in UI
- CSRF and security hardening review
- Route profile options (driving/walking/cycling)
- Unit and integration tests

## Screenshots

![landing-page](docs/images/landing-page.png)

![route-view](docs/images/route-view.png)

Recommended folder: `docs/images/`


## Author

Rohit Jain  
LinkedIn: https://www.linkedin.com/in/546-rohit-jain
