FROM node:20-alpine AS frontend-builder
WORKDIR /app/frontend
COPY package*.json tsconfig*.json tailwind.config.js index.html vite.config.ts components.json ./
COPY src ./src
COPY public ./public
RUN npm install
RUN npm run build

FROM node:20-alpine AS backend-builder
WORKDIR /app/backend
COPY proxy-server/package*.json ./
RUN npm install --omit=dev

FROM node:20-alpine
WORKDIR /app
COPY --from=backend-builder /app/backend/node_modules ./node_modules
COPY proxy-server/package.json ./
COPY proxy-server/server.js ./
COPY --from=frontend-builder /app/frontend/dist ./dist
EXPOSE 3000
ENV PORT=3000
CMD ["npm", "start"]
