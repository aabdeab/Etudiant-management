
FROM node:18-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build
FROM nginx:1.23.2-alpine AS production
RUN apk update && apk upgrade --no-cache
ENV NODE_ENV=production
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80 
CMD ["nginx", "-g", "daemon off;"]
