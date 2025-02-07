# Use Node.js as the base image
FROM node:22-alpine

# Install Git
RUN apk update && apk add git

# Set working directory
WORKDIR /workspace

# Install global dependencies for Angular
RUN npm install -g @angular/cli