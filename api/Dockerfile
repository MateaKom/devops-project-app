FROM node:22-alpine AS builder
WORKDIR /app
COPY package.json .
RUN npm install --production
FROM node:22-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --from=builder /app/node_modules ./node_modules
COPY src/ ./src/
USER appuser
EXPOSE 8080
CMD ["node", "src/server.js"]