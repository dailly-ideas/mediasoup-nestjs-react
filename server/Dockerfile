FROM node:18-bullseye

# Install dependencies for mediasoup and other required packages
RUN apt-get update && \
    apt-get install -y \
    python3 \
    python3-pip \
    build-essential \
    net-tools \
    procps \
    iproute2 \
    libssl-dev \
    pkg-config \
    gcc \
    g++ \
    make \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies with specific settings for mediasoup
ENV MEDIASOUP_BYPASS_WORKER_PREBUILT_DOWNLOAD="true"
RUN npm install --build-from-source

# Copy the rest of the application
COPY . .

# Build the application
RUN npm run build

# Create directory for mediasoup logs
RUN mkdir -p /var/log/mediasoup && \
    chmod -R 777 /var/log/mediasoup

# Set permissions for app directory
RUN chown -R node:node /app && \
    chmod -R 755 /app

# Switch to non-root user
USER node

# Expose the port the app runs on
EXPOSE 5001
# Expose ports for MediaSoup
EXPOSE 40000-40999/udp

# Command to run the application
CMD ["npm", "run", "start:prod"] 