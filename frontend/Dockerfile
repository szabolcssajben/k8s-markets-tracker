# syntax=docker.io/docker/dockerfile:1

FROM node:22-alpine AS base

WORKDIR /app

# Install dependencies
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* .npmrc* ./

# Install based on available lockfile
RUN \
    if [ -f yarn.lock ]; then yarn install; \
    elif [ -f package-lock.json ]; then npm install; \
    elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm install; \
    else echo "Lockfile not found." && npm install; \
    fi

# Disable telemetry in development
ENV NEXT_TELEMETRY_DISABLED=1

# Expose port
EXPOSE 3000

# Start development server with hot reloading
CMD ["sh", "-c", "if [ -f yarn.lock ]; then yarn dev; elif [ -f package-lock.json ]; then npm run dev; elif [ -f pnpm-lock.yaml ]; then pnpm run dev; else npm run dev; fi"]