# Base stage that uses the official python runtime base image
FROM python:3.11-slim AS base

RUN apt-get update && \
	apt-get install -y --no-install-recommends curl && \
	rm -rf /var/lib/apt/lists/*

# Application directory
WORKDIR /usr/local/app

# Install dependencies/requirements.txt
COPY requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Define a stage specifically for development, where it'll watch for filesystem changes
FROM base AS dev
RUN pip install watchdog
ENV FLASK_ENV=development
CMD ["python","app.py"]

# Define the final stage that will bundle the application for production
FROM base AS final

# Copy the code from the current folder to the workdir inside the container
COPY . .

# Make port 80 avaible for links and/or publish
EXPOSE 80

# Commands to run when launching the container
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0" ]
