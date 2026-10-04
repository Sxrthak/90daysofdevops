FROM python:3.12-slim

WORKDIR /app

# Install dependencies first so this layer is cached between builds
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the app code (this line was missing on my first attempt)
COPY . .

EXPOSE 80

ENTRYPOINT ["python", "run.py"]
