<h2>Project Overview</h2>

<p>
A fully containerized MEAN stack (MongoDB, Express, Angular, Node.js) application was developed to perform standard CRUD (Create, Read, Update, Delete) operations while demonstrating a modern DevOps workflow and cloud deployment strategy. The frontend, backend, and database are packaged into separate Docker containers and orchestrated using Docker Compose, creating a modular microservices-based architecture. Nginx is configured as a reverse proxy to serve the compiled Angular frontend and manage all incoming traffic through port 80. The application is deployed on an AWS EC2 Ubuntu instance, and a Jenkins CI/CD pipeline automates the entire deployment process. Whenever code is pushed to GitHub, Jenkins builds updated Docker images, pushes them to Docker Hub (groot321), and automatically deploys the latest containers to the AWS server without manual intervention. Multi-stage Docker builds are used to optimize image size and improve performance, and MongoDB is configured with Docker volumes to ensure data persistence even after container restarts, resulting in a stable and continuous delivery environment.
</p>

<h2>Technologies Used</h2>

<ul>
    <li><strong>Frontend:</strong> Angular 15, HTML/CSS, TypeScript</li>
    <li><strong>Backend:</strong> Node.js, Express.js</li>
    <li><strong>Database:</strong> MongoDB 4.4</li>
    <li><strong>Containerization:</strong> Docker, Docker Compose</li>
    <li><strong>Web Server / Proxy:</strong> Nginx (Alpine)</li>
    <li><strong>CI/CD:</strong> Jenkins</li>
    <li><strong>Cloud Infrastructure:</strong> AWS EC2 (Ubuntu AMI)</li>
    <li><strong>Container Registry:</strong> Docker Hub</li>
</ul>

<h2>Complete Development & Deployment Workflow</h2>

<h3>1. Infrastructure Setup (AWS EC2)</h3>

<ul>
    <li><strong>Instance Type:</strong> t2.medium</li>
    <li><strong>Operating System:</strong> Ubuntu AMI</li>
    <li><strong>Storage:</strong> 20 GB EBS volume</li>
</ul>

<p><strong>Security Group Configuration:</strong></p>
<ul>
    <li>Port 22 – SSH access</li>
    <li>Port 80 – Public web access</li>
    <li>Port 8080 – Jenkins access</li>
</ul>

<p>
Docker and Docker Compose were installed on the Ubuntu instance. Jenkins was also installed to manage the CI/CD automation.
</p>

<h3>Code Management</h3>

<ul>
    <li>The application source code is stored in a GitHub repository.</li>
    <li>Developers push updates to the repository.</li>
    <li>Jenkins continuously monitors the repository.</li>
    <li>Every code push automatically triggers the pipeline.</li>
</ul>

<h3>Continuous Integration (Build Process)</h3>

<h4>Checkout Code</h4>
<p>Jenkins pulls the latest source code from GitHub.</p>

<h4>Build Docker Images</h4>
<pre><code>docker compose build</code></pre>

<p>This builds Docker images for:</p>

<ul>
    <li>Angular Frontend (multi-stage build with Nginx)</li>
    <li>Node.js Backend</li>
    <li>MongoDB (if defined in compose)</li>
</ul>

<p>
The Angular build is compiled and served using Nginx for better performance and smaller image size.
</p>

<h3>Push Docker Images to Docker Hub</h3>

<p>Jenkins securely logs into Docker Hub using stored credentials.</p>

<pre><code>docker compose push</code></pre>

<p>
The images are pushed to the Docker Hub account.
</p>

<p>
This ensures the latest version of the application is available for deployment.
</p>

<h3>Continuous Deployment to EC2</h3>

<p>Once images are pushed successfully, Jenkins updates the live server.</p>

<p><strong>Pull Latest Images</strong></p>
<pre><code>docker compose pull</code></pre>

<p><strong>Stop Existing Containers</strong></p>
<pre><code>docker compose down</code></pre>

<p><strong>Start Updated Containers</strong></p>
<pre><code>docker compose up -d</code></pre>

<p>
Docker Compose recreates only the updated containers and keeps the internal networking intact.
</p>

<h3>Reverse Proxy Configuration</h3>

<ul>
    <li>Nginx is configured to listen on port 80.</li>
    <li>Only port 80 is exposed for public access.</li>
    <li>Backend and MongoDB ports are not exposed publicly.</li>
    <li>Users access the application using the EC2 Public IP.</li>
</ul>
