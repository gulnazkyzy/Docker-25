# stage - 1 install dependencies
FROM node:20-alpine AS deps 

WORKDIR /app

COPY package*.json ./

# create Node Modules folder
RUN npm install

# Stage -2 Build and compile
FROM node:20-alpine AS builder

WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules

COPY . .

RUN npm run build

# Stage - 3 Final stage

FROM node:20-alpine AS final  

WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/next.config.js ./

RUN chown -R node:node /app
USER node
EXPOSE 3000
CMD ["npm", "start"]