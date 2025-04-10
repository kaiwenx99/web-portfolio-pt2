# How to Run My Porfolio

This is a summary of steps to reload and restart this Flask app that is served with Gunicorn and Caddy on an Ubuntu server.

## If only modify app content, run step 1 and 4 only.

## 1. Activate the Virtual Environment

```bash
source vent/bin/activate
```

---

## 2. Install Python Dependencies

```bash
pip install Flask-Session Flask-Login
```

---

## 3. Disable the systemd Service Running Gunicorn

This app is auto-starting via systemd `flaskapp.service`:

### Stop the service:

```bash
sudo systemctl stop flaskapp
```

### Disable it from starting automatically:

```bash
sudo systemctl disable flaskapp
```

### Kill any remaining Gunicorn processes:

```bash
sudo pkill gunicorn
```

### Confirm port 5000 is free:

```bash
sudo lsof -i :5000
# (no output means the port is free)
```

---

## 4. Start Gunicorn Manually

From your Flask app directory:

```bash
./vent/bin/gunicorn app:app --bind 0.0.0.0:5000
```
