# Stage 1: The "Installer" or "Builder" Stage
# This stage installs dependencies and builds the React app.
FROM node:18-alpine AS installer

WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker's layer caching.
COPY package*.json ./

# Install project dependencies.
RUN npm install

# Copy the rest of the application's source code.
COPY . .

# Build the application for production.
RUN npm run build

# Stage 2: The "Deployer" or "Runner" Stage
# This stage takes the built artifacts and serves them with Nginx.
FROM nginx:latest AS deployer

# Copy only the built assets from the 'installer' stage.
COPY --from=installer /app/build /usr/share/nginx/html

# Expose port 80 to the outside world. Nginx's default port.
EXPOSE 80

# The default command for the nginx image is to start the server.
# CMD ["nginx", "-g", "daemon off;"]