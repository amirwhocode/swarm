# Frontend Framework Development Environment with Docker
This project sets up a development environment using Docker for four popular frontend frameworks: **React**, **Vue**, **Angular**, and **Svelte**. Each framework is configured with its own service in Docker, allowing you to quickly spin up isolated development environments.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Services Overview](#services-overview)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Access the Development Server](#access-the-development-servers)

## Prerequisites
- Docker (including Docker Compose) installed on your machine.
- The src folder is ignored by Git, so you'll need to create it manually before starting Docker. If you're not installing the required framework (which Docker Compose needs), Docker will crash. In that case, just create an empty src folder in each directory.

## Services Overview
This project defines the following services in a `docker-compose.yml` file:

1. **React**
   - **Volume**: Binds the local `./react/src` folder to `/workspace` in the container.

2. **Vue**
   - **Volume**: Binds the local `./vue/src` folder to `/workspace` in the container.

3. **Angular**
   - **Volume**: Binds the local `./angular/src` folder to `/workspace` in the container.

4. **Svelte**
   - **Volume**: Binds the local `./svelte/src` folder to `/workspace` in the container.

## Getting Started

### 1. Clone the repository (if not already done)
```
git clone https://github.com/amirwhocode/swarm.git
cd swarm
```
### 2. Build the image
```
docker-compose build --no-cache
```
### 3. Start the container and then access the container's command line
```
docker-compose up
```

## Development Workflow
### React
There are two ways to create a React app. The `create-react-app` command is already installed in the container, and you can use this to create a new React app:
1. **Using `create-react-app` command**:
```
create-react-app <project-name>
```
2. **Using Vite**:
```
npm create vite@latest
```
### Vue
1. **Using Vue CLI**:
```
vue create <project-name>
cd <project-name>
npm run serve
```
2. **build setup based on Vite**:
```
npm create vue@latest
cd <your-project-name>
npm install
npm run dev
```
3. **For installing Quasar**:
```
npm init quasar@latest
cd <your-project-name>
quasar dev
```
### Angular
* **using Angular CLI**
```
ng new <project-name>
cd <project-name>
ng serve --host 0.0.0.0
```
### Svelte
* **using SvelteKit powered by Vite**:
```
npx sv create <project-name>
cd <project-name>
npm install
npm run dev
```
## Access the Development Servers
Once the containers are running and you have successfully installed the chosen framework (React, Vue, Angular, or Svelte), you can access the frontend applications in your browser:

### React
   - **Default**: [http://localhost:53000](http://localhost:53000)
   - **With Vite**: [http://localhost:55173](http://localhost:55173) (if applicable)

### Vue
   - **Vue CLI**: [http://localhost:68080](http://localhost:68080)
   - **With Vite**: [http://localhost:65173](http://localhost:65173) (if applicable)

### Angular
   - **Angular CLI**: [http://localhost:54200](http://localhost:54200)

### Svelte
   - **SvelteKit powered by Vite**: [http://localhost:45173](http://localhost:45173)

## Troubleshooting
### If you installed React with Vite
If you're using **Vite** to create a React app, you may encounter issues accessing the app from your host browser. To fix this, you need to update the `vite.config.ts` file to ensure the server is accessible outside the container.

1. **Modify `vite.config.ts`**:
Open the `vite.config.ts` file in your React project and add or modify the server configuration:
```
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
   plugins: [react()],
   server: {
      host: '0.0.0.0',  // Allows access from outside the container
      port: 5173,        // Explicitly setting the port (not strictly necessary but good practice)
      }
})
``` 
###  App Not Accessible from Host in Docker
#### Problem:
By default, development servers (e.g., Angular, React) bind to localhost (127.0.0.1), making the app accessible only within the container, not from the host.
#### Cause:
Binding to localhost restricts access to the container. To access the app from the host, the server needs to listen on all network interfaces (0.0.0.0).
#### Solution:
Temporary fix: Run the app with --host 0.0.0.0 (e.g., ng serve --host 0.0.0.0 or npm start --host 0.0.0.0).
#### Permanent fix:
Modify your framework's config to bind to 0.0.0.0 by default (e.g., "host": "0.0.0.0" in angular.json or "HOST": "0.0.0.0" in .env for React).
#### Why It Works:
Binding to 0.0.0.0 allows the server to accept connections from both the container and the host machine.