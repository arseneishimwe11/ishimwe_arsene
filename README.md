# Express.js with Docker and NGINX Reverse Proxy

![Node.js](https://img.shields.io/badge/Node.js-22-green)
![Express.js](https://img.shields.io/badge/Express.js-4.18.2-blue)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![NGINX](https://img.shields.io/badge/NGINX-Reverse%20Proxy-green)

A complete DevOps demonstration project showcasing a simple Express.js web server containerized with Docker and configured with NGINX as a reverse proxy.

## 🚀 Features

- **Express.js Web Server**: Simple REST API responding with "Hello, DevOps!"
- **Docker Containerization**: Multi-stage Docker build with security best practices
- **NGINX Reverse Proxy**: Load balancing and request forwarding
- **Health Checks**: Built-in health monitoring for both services
- **Security**: Non-root user, security headers, and proper networking
- **Production Ready**: Graceful shutdown, logging, and error handling

## 📁 Project Structure

```
express-docker-nginx/
├── 📄 app.js                 # Express.js application
├── 📄 package.json           # Node.js dependencies and scripts
├── 📄 Dockerfile             # Docker container configuration
├── 📄 nginx.conf             # NGINX reverse proxy configuration
├── 📄 docker-compose.yml     # Multi-container orchestration
├── 📄 .gitignore             # Git ignore rules
├── 📄 .dockerignore          # Docker ignore rules
├── 📄 README.md              # Project documentation
├── 🔧 build.sh               # Build automation script
└── 🔧 run.sh                 # Run automation script
```

## 🛠️ Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v18 or higher)
- **Docker** (v20.10 or higher)
- **Docker Compose** (v2.0 or higher)
- **Git** (for version control)

### Verify Installation

```bash
node --version
docker --version
docker-compose --version
```

## ⚡ Quick Start

### Option 1: Using Docker Compose (Recommended)

1. **Clone and navigate to the project:**
   ```bash
   git clone <repository-url>
   cd express-docker-nginx
   ```

2. **Start the application:**
   ```bash
   docker-compose up -d
   ```

3. **Access the application:**
   - **Via NGINX Proxy**: http://localhost
   - **Direct Express.js**: http://localhost:3000

4. **Stop the application:**
   ```bash
   docker-compose down
   ```

### Option 2: Using Build Scripts

1. **Make scripts executable:**
   ```bash
   chmod +x build.sh run.sh
   ```

2. **Build the application:**
   ```bash
   ./build.sh
   ```

3. **Run the application:**
   ```bash
   ./run.sh
   ```

## 🔧 Manual Setup

### Step 1: Install Dependencies

```bash
npm install
```

### Step 2: Run Locally (Development)

```bash
npm start
# or for development with auto-reload
npm run dev
```

### Step 3: Build Docker Image

```bash
docker build -t express-docker-nginx .
```

### Step 4: Run Express.js Container

```bash
docker run -d --name express-app -p 3000:3000 express-docker-nginx
```

### Step 5: Run NGINX Container

```bash
docker run -d --name nginx-proxy \
  -p 80:80 \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
  --link express-app \
  nginx:alpine
```

## 🧪 Testing & Verification

### Basic Functionality Tests

```bash
# Test via NGINX reverse proxy (should return "Hello, DevOps!")
curl http://localhost

# Test direct Express.js access
curl http://localhost:3000

# Test Express.js health endpoint
curl http://localhost:3000/health

# Test NGINX health endpoint
curl http://localhost/nginx-health
```

### Advanced Testing

```bash
# Test with headers
curl -H "Accept: application/json" http://localhost/health

# Test response time
curl -w "@curl-format.txt" -o /dev/null -s http://localhost

# Load testing (if you have ab installed)
ab -n 1000 -c 10 http://localhost/
```

### Expected Responses

**Main Endpoint (`GET /`):**
```
Hello, DevOps!
```

**Health Endpoint (`GET /health`):**
```json
{
  "status": "OK",
  "timestamp": "2024-01-15T10:30:00.000Z",
  "port": 3000
}
```

## 📊 Monitoring & Logs

### View Container Status

```bash
# Check all containers
docker-compose ps

# Check specific service
docker-compose ps express-app
```

### View Logs

```bash
# View all logs
docker-compose logs

# Follow logs in real-time
docker-compose logs -f

# View specific service logs
docker-compose logs express-app
docker-compose logs nginx
```

### Health Checks

```bash
# Check container health
docker inspect --format='{{.State.Health.Status}}' express-app

# View health check logs
docker inspect --format='{{range .State.Health.Log}}{{.Output}}{{end}}' express-app
```

## 🔒 Security Features

- **Non-root user**: Application runs as `nodejs` user (UID 1001)
- **Security headers**: X-Frame-Options, X-XSS-Protection, etc.
- **Minimal base image**: Using Alpine Linux for smaller attack surface
- **Network isolation**: Services communicate through Docker network
- **Health monitoring**: Built-in health checks for service reliability

## 🚀 Production Deployment

### Environment Variables

Create a `.env` file for production:

```env
NODE_ENV=production
PORT=3000
NGINX_PORT=80
```

### Production Docker Compose

```bash
# Use production configuration
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

### Scaling

```bash
# Scale Express.js instances
docker-compose up -d --scale express-app=3
```

## 🛠️ Development

### Local Development Setup

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Start development server:**
   ```bash
   npm run dev
   ```

3. **Run tests:**
   ```bash
   npm test
   ```

### Adding New Features

1. Modify `app.js` for new endpoints
2. Update `package.json` if new dependencies are needed
3. Rebuild Docker image: `docker-compose build`
4. Test changes: `docker-compose up -d`

## 🐛 Troubleshooting

### Common Issues

**Port Already in Use:**
```bash
# Kill processes using port 80 or 3000
sudo lsof -ti:80 | xargs kill -9
sudo lsof -ti:3000 | xargs kill -9
```

**Container Won't Start:**
```bash
# Check container logs
docker-compose logs express-app

# Check Docker daemon
sudo systemctl status docker
```

**NGINX 502 Bad Gateway:**
```bash
# Check if Express.js container is running
docker-compose ps express-app

# Verify network connectivity
docker-compose exec nginx ping express-app
```

### Debug Mode

```bash
# Run with debug output
DEBUG=* docker-compose up

# Access container shell
docker-compose exec express-app sh
docker-compose exec nginx sh
```

## 📈 Performance Optimization

### NGINX Optimizations

- Gzip compression enabled
- Keep-alive connections
- Proper buffer sizes
- Static file caching

### Node.js Optimizations

- Production mode (`NODE_ENV=production`)
- Graceful shutdown handling
- Memory leak prevention
- Proper error handling

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -am 'Add feature'`
4. Push to branch: `git push origin feature-name`
5. Submit a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Express.js team for the excellent web framework
- Docker team for containerization technology
- NGINX team for the powerful reverse proxy
- Node.js community for continuous innovation
---

**Made with ❤️ for DevOps learning and demonstration**

### 🏷️ Tags

`nodejs` `express` `docker` `nginx` `devops` `containerization` `reverse-proxy` `microservices` `web-development`