# syntax=docker/dockerfile:1
FROM node:20-alpine AS builder

WORKDIR /usr/src/app

COPY package*.json ./
RUN  --ignore-scripts

COPY tsconfig*.json ./
COPY nest-cli.json ./
COPY src ./src
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /usr/src/app
ENV NODE_ENV=production

COPY package*.json ./
RUN npm ci --production --ignore-scripts

COPY --from=builder /usr/src/app/dist ./dist
RUN chown -R node:node /usr/src/app
USER node

EXPOSE 3000
CMD ["npm", "run", "start:prod"]
