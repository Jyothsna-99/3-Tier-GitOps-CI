# Stage 1: Build React App
FROM node:20-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm config set registry https://registry.npmjs.org/ \
 && npm config set fetch-retries 5 \
 && npm config set fetch-retry-factor 2 \
 && npm config set fetch-timeout 600000 \
 && npm cache clean --force \
 && npm install --omit=dev
 
COPY . .

# Optional: pass REACT_APP_API as build arg
ARG REACT_APP_API
ENV REACT_APP_API=$REACT_APP_API

RUN npm run build

# Stage 2: Serve with NGINX
FROM nginx:alpine

# 🟢 Copy correct default.conf to override nginx default
COPY default.conf /etc/nginx/conf.d/default.conf

# 🟢 Copy React build output
COPY --from=build /app/build /usr/share/nginx/html

# 🔒 Prevent directory listings and ensure correct permissions
RUN chmod -R 755 /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
