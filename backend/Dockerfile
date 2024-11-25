# Stage 1:
    FROM python:3.10-alpine AS builder
    RUN pip install --no-cache-dir poetry
    WORKDIR /app
    COPY pyproject.toml poetry.lock ./
    RUN poetry config virtualenvs.create false && poetry install --no-dev --no-interaction --no-ansi
    COPY . .
    
    # Stage 2: 
    FROM python:3.10-alpine
    RUN apk add --no-cache libpq
    WORKDIR /app
    COPY --from=builder /app /app
    COPY ./prestart.sh /prestart.sh
    EXPOSE 8000
    CMD ["sh", "-c", "bash /prestart.sh && uvicorn app.main:app --host 0.0.0.0 --port 8000"]