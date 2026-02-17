<h1>Project 2 — Dockerize a Node.js App (Beginner → Solid Foundations)</h1>

<p>This project demonstrates how to containerize a simple Node.js Express API using Docker. You will learn image building, layer caching, and container execution.</p>

<hr>

<h2>Goal</h2>
<p>Run a Node.js Express app inside Docker and access it from a browser.</p>

<hr>

<h2>Step 0 — Check Node (Optional)</h2>
<pre><code>node -v</code></pre>

<hr>

<h2>Step 1 — Create Project Folder</h2>
<pre><code>mkdir docker-node-app
cd docker-node-app</code></pre>

<hr>

<h2>Step 2 — Initialize Node App</h2>
<pre><code>npm init -y
npm install express</code></pre>

<hr>

<h2>Step 3 — Create index.js</h2>
<pre><code>touch index.js</code></pre>

<pre><code>const express = require("express");
const app = express();

const PORT = 3000;

app.get("/", (req, res) => {
  res.send("Hello from Dockerized Node.js");
});

app.listen(PORT, "0.0.0.0", () => {
  console.log(`Server running on port ${PORT}`);
});</code></pre>

<p><strong>Note:</strong> Using <code>0.0.0.0</code> allows Docker to expose the port.</p>

<hr>

<h2>Step 4 — Create Dockerfile</h2>
<pre><code>touch Dockerfile</code></pre>

<pre><code># Base image
FROM node:20-alpine

# Working directory
WORKDIR /app

# Copy dependency files first (layer caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy remaining files
COPY . .

# Expose app port
EXPOSE 3000

# Start app
CMD ["node", "index.js"]</code></pre>

<hr>

<h2>Step 5 — Create .dockerignore</h2>
<pre><code>touch .dockerignore</code></pre>

<pre><code>node_modules
npm-debug.log
.git</code></pre>

<p>Prevents unnecessary files from being added to the Docker image.</p>

<hr>

<h2>Project Structure</h2>
<pre><code>docker-node-app/
├── index.js
├── package.json
├── package-lock.json
├── Dockerfile
└── .dockerignore</code></pre>

<hr>

<h2>Step 6 — Build Image</h2>
<pre><code>docker build -t node-docker-app .</code></pre>

<pre><code>docker images</code></pre>

<hr>

<h2>Step 7 — Run Container</h2>
<pre><code>docker run -d -p 3000:3000 --name node-container node-docker-app</code></pre>

<pre><code>docker ps</code></pre>

<hr>

<h2>Step 8 — Test in Browser</h2>
<pre><code>http://localhost:3000</code></pre>

<p>Expected output:</p>
<pre><code>Hello from Dockerized Node.js</code></pre>
