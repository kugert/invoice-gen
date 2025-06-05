## ------------------------------ Builder Stage ------------------------------ ##
FROM python:3.12-bookworm AS builder

# Set the working directory
RUN apt-get update && apt-get install --no-install-recommends -y \
        build-essential \
        libpq-dev \
        postgresql-client \
        ca-certificates \
    && apt-get clean  \
    && rm -rf /var/lib/apt/lists/*

# Install uv package manager
ADD https://astral.sh/uv/install.sh /install.sh
RUN chmod 755 /install.sh && /install.sh && rm /install.sh

# Setup uv path correctly
ENV PATH="/root/.local/bin:${PATH}"

WORKDIR /app

COPY ./pyproject.toml .

RUN uv sync

## ------------------------------ Production Stage ------------------------------ ##
FROM python:3.12-slim-bookworm AS production

ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1

RUN useradd --create-home appuser
USER appuser

WORKDIR /app
COPY ./src .
COPY --from=builder /app/.venv .venv

ENV PATH="/app/.venv/bin:${PATH}"

EXPOSE 8000

CMD ["fastapi", "run", "main.py"]
