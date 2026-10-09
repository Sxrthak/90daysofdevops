# Node.js 22 LTS on Alpine Linux (small base image)
FROM node:22-alpine

# Run in production mode 
ENV NODE_ENV=production

WORKDIR /app

COPY package.json package-lock.json ./


RUN npm ci --omit=dev && npm cache clean --force


COPY --chown=node:node . .


USER node


EXPOSE 5000


CMD ["node", "src/server.js"]
