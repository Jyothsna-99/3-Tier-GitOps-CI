FROM node:22-alpine

WORKDIR /app
COPY package*.json ./
RUN npm config set registry https://registry.npmjs.org/ \
 && npm config set fetch-retries 5 \
 && npm config set fetch-retry-factor 2 \
 && npm config set fetch-timeout 600000 \
 && npm cache clean --force \
 && npm install --omit=dev 

COPY . .
EXPOSE 5000
CMD ["node", "app.js"]

