FROM python:3.9-slim

ENV PYTHONUNBUFFERED=1
ENV DEBUG=0

WORKDIR /django

# Install system dependencies (required for some Python packages)
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc python3-dev && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Collect static files
RUN python manage.py collectstatic --noinput    

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
