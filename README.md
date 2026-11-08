# Flask AWS Lightsail Deployment

This project is prepared for deployment to an AWS Lightsail Ubuntu instance using Gunicorn and Nginx.

## Local development

```bash
python -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
python init_db.py
python app.py
```

## Production-style server run

```bash
gunicorn --config gunicorn.conf.py app:app
```

## Systemd service

Use the sample service at `deployment/flaskapp.service` on your server:

```bash
sudo cp deployment/flaskapp.service /etc/systemd/system/flaskapp.service
sudo systemctl daemon-reload
sudo systemctl start flaskapp
sudo systemctl enable flaskapp
```

## Nginx reverse proxy

Use the sample proxy at `deployment/nginx-flaskapp.conf`:

```bash
sudo cp deployment/nginx-flaskapp.conf /etc/nginx/sites-available/flaskapp
sudo ln -s /etc/nginx/sites-available/flaskapp /etc/nginx/sites-enabled/flaskapp
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

This configuration exposes the Flask app through port 80 and keeps Gunicorn bound to the local loopback address on port 8000.
