# Stage 1: Build the application
FROM node:18 AS build

# Set the working directory to /opt
WORKDIR /opt

# Copy package.json and package-lock.json first to leverage Docker cache
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application (optional, if using a build step)
RUN npm run build

# Stage 2: Create a smaller image for the runtime
FROM node:18-slim

# Set the working directory to /opt
WORKDIR /opt

# Copy only the necessary files from the build stage
COPY --from=build /opt ./

# Install only production dependencies
RUN npm install --only=production

# Expose the port that the app runs on
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
