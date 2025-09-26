# Dockerfile
FROM python:3.11-slim

# keep python output unbuffered (logs show immediately)
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# copy only requirements first to use Docker cache
COPY requirements.txt .

# install system deps (for building any wheels) and python deps
RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential \
    && pip install --upgrade pip \
    && pip install -r requirements.txt \
    && apt-get purge -y --auto-remove build-essential \
    && rm -rf /var/lib/apt/lists/*

# copy app source
COPY . .

# expose container port (match with gunicorn bind below)
EXPOSE 8080

# run with gunicorn; expects object named `app` inside app.py (i.e. app = Flask(...))
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:8080", "--workers", "3", "--threads", "2", "--timeout", "120"]


