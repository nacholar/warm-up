# Base image
FROM node:lts-alpine3.16 as builder

# Create app directory
WORKDIR /app/builder

# A wildcard is used to ensure both package.json AND package-lock.json are copied
COPY package*.json ./

# Install app dependencies
RUN npm install

# Bundle app source
COPY . .

# Creates a "dist" folder with the production build
RUN npm run build

FROM node:lts-alpine3.16 as deploy

WORKDIR /app

COPY --from=builder /app/builder/dist ./dist
COPY --from=builder /app/builder/package*.json ./

RUN npm install --only=production
# Start the server using the production build
CMD [ "node", "dist/main" ]
