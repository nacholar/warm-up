FROM node:lts-alpine3.16 as deps 

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

FROM node:lts-alpine3.16 as builder

WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
# Copying source files
COPY . .

# Building app
RUN npm run build

RUN rm -rf node_modules
RUN npm install --only=production

FROM node:lts-alpine3.16 as deploy

WORKDIR /app
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/public ./public/
COPY --from=builder /app/.next ./.next

EXPOSE 3000

# Running the app
CMD [ "npm", "start"]
