# Docker from Start to Finish 🐳

Welcome to the **Docker from Start to Finish** tutorial! In this lab, you'll learn how to take a simple Node.js application that connects to a MongoDB database, containerize it using Docker, and orchestrate multiple containers using Docker Compose.

---

## 🎯 What we are building
We have a beautiful "User Profile" web application built with **Node.js, Express, and Vanilla HTML/CSS** (featuring a modern glassmorphism design). 
The app stores user profile data in a **MongoDB** database.

## 📋 Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed.
- [Node.js](https://nodejs.org/) installed (only needed for Part 1).

---

## 📦 Part 1: Running Locally (Without Docker)
Before containerizing an app, it's essential to understand how it runs normally.

1. **Start the database locally:**
   Since we don't have MongoDB installed on our machine, we can cheat a little and use Docker just for the database (we'll learn how this works later!):
   ```bash
   docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb mongo
   ```

2. **Start the Node.js Application:**
   Open a terminal in the `app` folder and run:
   ```bash
   cd app
   npm install
   npm start  # Or run: node server.js
   ```

3. **View the App:**
   Open your browser and navigate to `http://localhost:3000`. You'll see the stunning glassmorphism profile page! Try editing the profile. 
   *(Stop the app and the database container before moving to Part 2).*

---

## 🏗️ Part 2: The Dockerfile (Containerizing the App)
Instead of relying on having Node.js installed locally, we can package our application and its environment into a **Docker Image**. We do this using a `Dockerfile`.

Open `app/Dockerfile` and examine it. Here's what the instructions mean:

- `FROM node:18-alpine`: The base image. It's like buying a computer with Node.js already installed. `alpine` means it's a very small, lightweight version of Linux.
- `WORKDIR /usr/src/app`: We create a folder inside the container and CD into it.
- `COPY package*.json ./`: We copy our dependency files first.
- `RUN npm install`: We install the dependencies *inside* the container.
- `COPY . .`: We copy the rest of our application code into the container.
- `EXPOSE 3000`: We document that this container will listen on port 3000.
- `CMD ["node", "server.js"]`: The default command to run when the container starts.

---

## 🚀 Part 3: Building & Running the Image
Now let's build our custom image from the `Dockerfile`.

1. **Build the Image:**
   Make sure you are in the `app` directory (where the Dockerfile is) and run:
   ```bash
   docker build -t dockerlabs-app .
   ```
   *- `docker build`: Command to build an image.*
   *- `-t dockerlabs-app`: Tags (names) the image "dockerlabs-app".*
   *- `.`: The build context (current directory).*

2. **Run the Image:**
   ```bash
   docker run -p 3000:3000 dockerlabs-app
   ```
   Wait! If you go to `http://localhost:3000`, the app will load, but if you try to save a profile, it will fail. Why? Because the Node container is trying to reach a MongoDB database at `localhost:27017`, but "localhost" inside the container means the container itself! 

   We need a way to connect multiple containers together. Enter **Docker Compose**.

---

## 🐙 Part 4: Docker Compose (Orchestrating Containers)
Docker Compose allows us to define and run multi-container Docker applications using a single YAML file (`docker-compose.yaml`).

Open `docker-compose.yaml`. You will see three services defined:
1. **`app`**: Our custom Node.js application. Notice how we pass `MONGO_URL` as an environment variable so it knows where to find the database!
2. **`mongodb`**: The official MongoDB database.
3. **`mongo-express`**: A web-based administrative interface for MongoDB.

**Running the whole stack:**
Go back to the root `dockerlabs` folder and run:
```bash
docker-compose up -d
```
*- `-d`: Runs the containers in detached mode (in the background).*

Now visit:
- App: `http://localhost:3000`
- Database Admin (Mongo-Express): `http://localhost:8080` (Username: `admin`, Password: `password`)

---

## 💾 Part 5: Volumes & Networks
How does data survive if a container is destroyed? 

**Volumes:**
In `docker-compose.yaml`, look at the `mongodb` service. We defined:
```yaml
volumes:
  - mongo-data:/data/db
```
This tells Docker to create a persistent storage volume named `mongo-data` on your host machine and mount it to `/data/db` inside the container. Even if the MongoDB container is deleted, your data remains safe in the volume!

**Networks:**
Did you notice how we connected the app to the database?
`MONGO_URL=mongodb://admin:password@mongodb:27017`
We used the hostname `mongodb` instead of an IP address! Docker Compose automatically creates a custom virtual network for your app and provides built-in DNS resolution. The service name becomes the hostname.

---

## 🛑 Clean Up
When you are done, you can stop and remove all containers, networks, and volumes created by Compose:
```bash
docker-compose down
```
If you want to delete the persistent volume too (wiping the database):
```bash
docker-compose down -v
```

### 🎉 Congratulations! You've gone from zero to hero with Docker and Docker Compose! 
Don't forget to check out `DOCKER_COMMANDS.md` for a handy cheatsheet of everything you learned!
