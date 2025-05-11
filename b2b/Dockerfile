FROM python:3.9-slim

ENV PYTHONUNBUFFERED=1
ENV DEBUG=0

WORKDIR /django

# Install system dependencies (required for some Python packages)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    gcc \
    python3-dev \
    dnsutils \
    # Required for psycopg2 (PostgreSQL)
    libpq-dev \  
    && rm -rf /var/lib/apt/lists/* 

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Collect static files
RUN python manage.py collectstatic --noinput  

# Create directory for PostgreSQL socket
RUN mkdir -p /var/run/postgresql && chown -R 1000:1000 /var/run/postgresql



# Health check (optional but recommended)
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD python manage.py check --database default || exit 1

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
