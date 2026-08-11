# Complete Deployment Guide for Studentfacial (Flask Facial Recognition System)

This guide provides instructions for deploying the **Studentfacial** Flask application to production app hosting platforms including **Render**, **Railway**, **Docker**, and **Local Network Hosting**.

---

## 🏗️ Project Architecture & Deployment Requirements

- **Backend Framework**: Python Flask (WSGI app)
- **Production Server**: Gunicorn / Waitress
- **Computer Vision**: OpenCV (`opencv-contrib-python`) & Haar Cascade Classifier
- **Database**: SQLite (`students.db`)
- **Port**: Configured via `PORT` environment variable (defaults to `5000`)

---

## 🚀 Option 1: Deploy on Render (Recommended Cloud App Host)

Render provides free hosting for Python Flask web applications and Docker containers.

### Method A: Web Service Deployment (Standard Python)
1. Push this repository to **GitHub** or **GitLab**.
2. Log into [Render Dashboard](https://dashboard.render.com/) and click **New +** > **Web Service**.
3. Select your `Studentfacial` repository.
4. Render will automatically detect the settings from `render.yaml`:
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn wsgi:app`
5. Click **Create Web Service**.

### Method B: Docker Web Service Deployment (Ensures OpenCV OS Binaries)
1. On Render, click **New +** > **Web Service**.
2. Select your repository.
3. Set **Runtime / Environment** to `Docker`.
4. Render will detect the included `Dockerfile` and build all required OpenCV Linux libraries (`libgl1-mesa-glx`, `libglib2.0-0`) automatically.
5. Click **Create Web Service**.

---

## 🐳 Option 2: Deploy using Docker & Docker Compose

Docker containerization packages the Python environment, OpenCV system dependencies, and Flask server into a single container.

### Local Docker Build & Run:
```bash
# Build the Docker image
docker build -t studentfacial .

# Run container on port 5000
docker run -p 5000:5000 -e SECRET_KEY="your_secret_key" studentfacial
```

### One-Command Deployment with Docker Compose:
```bash
docker-compose up -d
```
Access the application at `http://localhost:5000`.

---

## 🚂 Option 3: Deploy on Railway / Fly.io / Heroku

### Deploying to Railway:
1. Connect your GitHub repository to [Railway](https://railway.app/).
2. Railway will read `Procfile` and `requirements.txt` automatically.
3. Set environment variable `PORT` (Railway provides `$PORT` automatically).

### Deploying to Heroku:
```bash
# Login and create app
heroku create studentfacial-app

# Deploy code
git push heroku main
```

---

## 🌐 Option 4: Local Network Hosting (School/Lab/College Deployment)

To host the system on your local Wi-Fi or LAN so multiple devices (laptops, mobiles, tablets) can access the dashboard and webcam attendance system:

1. **Find your Local IP Address**:
   - On Windows: Open Command Prompt and run `ipconfig` (Look for `IPv4 Address`, e.g., `192.168.1.15`).
2. **Start the Flask Application**:
   ```bash
   # Using Python directly
   python app.py

   # Or using Waitress WSGI server (Production on Windows)
   waitress-serve --port=5000 wsgi:app
   ```
3. **Access from Any Device on the Network**:
   Open browser on any laptop/phone connected to the same Wi-Fi:
   `http://192.168.1.15:5000`

---

## 🔐 Environment Variables Summary

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `PORT` | `5000` | Port for Flask web server |
| `SECRET_KEY` | `supersecretkey_studentfacial_2026` | Flask session encryption key |
| `FLASK_ENV` | `production` | Flask environment mode |
| `FLASK_DEBUG` | `False` | Enable/Disable debug mode |

---

## 📂 File Map of Deployment Resources

- `wsgi.py`: Production WSGI launcher ([wsgi.py](file:///d:/SEM%206/Adlab/Studentfacial/wsgi.py))
- `Procfile`: Gunicorn runner for Heroku/Render/Railway ([Procfile](file:///d:/SEM%206/Adlab/Studentfacial/Procfile))
- `Dockerfile`: Multi-stage Docker image definition ([Dockerfile](file:///d:/SEM%206/Adlab/Studentfacial/Dockerfile))
- `docker-compose.yml`: Multi-container orchestrator ([docker-compose.yml](file:///d:/SEM%206/Adlab/Studentfacial/docker-compose.yml))
- `render.yaml`: Render deployment blueprint ([render.yaml](file:///d:/SEM%206/Adlab/Studentfacial/render.yaml))
- `.env.example`: Environment variables template ([.env.example](file:///d:/SEM%206/Adlab/Studentfacial/.env.example))
