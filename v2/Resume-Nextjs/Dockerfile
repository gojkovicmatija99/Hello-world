# Stage 1: Build
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy all source code
COPY . .

# Build Next.js app (production build)
RUN npm run build

# -----------------------------
# Stage 2: Run
# -----------------------------
FROM node:18-alpine AS runner

WORKDIR /app
ENV NODE_ENV=production

# Copy files from builder
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./

# Expose port
EXPOSE 3000

# Start Next.js production server with Turbopack (optional)
# CMD ["npx", "next", "start", "--turbo"]

# Or standard production start
CMD ["npx", "next", "start", "-p", "3000"]
