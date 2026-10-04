# Day 1: Containerise a Flask app and deploy it on AWS EC2

I packaged a small Flask app into a Docker image and ran it on an Ubuntu EC2 instance, where it was reachable from a browser on port 80.

> **Credit:** the Flask app and HTML page come from [LondheShubham153/flask-app-ecs](https://github.com/LondheShubham153/flask-app-ecs) by Shubham Londhe (TrainWithShubham). I wrote the Dockerfile and did the EC2 deployment as part of my learning.

## Project structure

```
day-01-flask-docker-ec2/
├── app.py             # Flask routes: / and /health
├── run.py             # Starts the server on 0.0.0.0:80
├── requirements.txt   # flask, werkzeug
├── templates/
│   └── index.html
├── Dockerfile
└── .dockerignore
```

## Run it locally

```bash
docker build -t flask-app .
docker run -d -p 80:80 --name flask-app flask-app
```

Then open http://localhost. The health check is at http://localhost/health.

## Deploy on EC2

1. Launch an Ubuntu EC2 instance. In the security group, allow:
   - **22 (SSH)** from your own IP only
   - **80 (HTTP)** from anywhere
2. Connect to it:
   ```bash
   ssh -i <your-key>.pem ubuntu@<EC2-public-IP>
   ```
3. Install Docker:
   ```bash
   sudo apt update && sudo apt install -y docker.io
   sudo usermod -aG docker $USER   # log out and back in after this
   ```
4. Copy this folder to the server (or `git clone` the repo), then build and run:
   ```bash
   docker build -t flask-app .
   docker run -d -p 80:80 --name flask-app flask-app
   ```
5. Open `http://<EC2-public-IP>` in a browser.

Stop or terminate the instance when you're done so it doesn't keep charging you.

## What broke, and how I fixed it

| Error | Cause | Fix |
|---|---|---|
| `pip install` failed for Flask 3.1 | Base image was Python 3.7; Flask 3.1 needs Python 3.9+ | Use `python:3.12-slim` |
| Container exited straight away, `can't open file '/app/run.py'` | I forgot `COPY . .`, so the image had no app code | Add `COPY . .` after installing requirements |
| `invalid reference format` on build | `FROM python: 3.12-slim` had a space after the colon | `FROM python:3.12-slim` |

## Lessons

1. Read the error message before you start guessing.
2. `docker logs <container>` tells you why a container died.
3. Check your prompt: are you on your laptop or on the server?
