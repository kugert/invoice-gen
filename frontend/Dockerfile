# ------------------------------ Build Stage ------------------------------ #
FROM node:20-slim AS builder

WORKDIR /app

COPY package*.json ./
COPY tsconfig*.json ./
COPY vite.config.* ./
COPY public ./public
COPY src ./src

RUN npm install
RUN npm run build

# ------------------------------ NGINX Stage ------------------------------ #
FROM nginx:alpine AS production

# Copy built assets from builder
COPY --from=builder /app/dist /usr/share/nginx/html

# Replace default NGINX config if needed
# COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
