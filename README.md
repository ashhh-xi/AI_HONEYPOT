# AI Honeypot - Beelzebub

A sophisticated honeypot system that emulates multiple network services to detect and analyze malicious activities. This project uses Docker containers to run various honeypot services including SSH, HTTP, MySQL, and TCP services with RabbitMQ integration for event logging and Prometheus for metrics.

## 📁 Project Structure

```
honeypot_clean/
├── configurations/           # Configuration files
│   ├── beelzebub.yaml       # Main honeypot configuration
│   ├── honey.yaml           # Alternative configuration
│   └── services/            # Service-specific configurations
│       ├── http-80.yaml     # WordPress 6.0 HTTP service
│       ├── http-8080.yaml   # Apache 401 HTTP service
│       ├── http-8081.yaml   # Test HTTP service
│       ├── mysql-3306.yaml  # MySQL 8.0.29 service
│       ├── ssh-22.yaml      # SSH interactive service
│       ├── ssh-2222.yaml    # SSH ChatGPT service
│       └── tcp-3306.yaml    # Generic TCP service
├── protocols/               # Protocol implementations
│   ├── protocol_manager.go  # Manages different protocols
│   └── strategies/          # Protocol-specific strategies
│       ├── HTTP/           # HTTP protocol handling
│       ├── MySQL/          # MySQL protocol handling
│       ├── SSH/            # SSH protocol handling
│       └── TCP/            # TCP protocol handling
├── plugins/                 # Plugin system
│   ├── beelzebub-cloud.go  # Cloud integration
│   └── llm-integration.go  # LLM integration
├── builder/                 # Application builder
│   ├── builder.go          # Main builder logic
│   └── director.go         # Builder director pattern
├── parser/                  # Configuration parsing
│   └── configurations_parser.go
├── tracer/                  # Event tracing
│   └── tracer.go
├── historystore/           # Event history storage
│   └── history_store.go
├── integration_test/       # Integration tests
├── beelzebub-chart/        # Kubernetes Helm chart
├── docker-compose.yml      # Docker services configuration
├── Dockerfile             # Container build instructions
├── main.go                # Application entry point
├── go.mod                 # Go module dependencies
├── go.sum                 # Go module checksums
└── Makefile               # Build and deployment commands
```

## 🚀 Quick Start

### Prerequisites

- Docker Desktop installed and running
- Git
- At least 2GB RAM available for containers

### Step 1: Clone and Setup

```bash
# Clone the repository
git clone <your-repo-url>
cd honeypot_clean

# Create .env file
touch .env
```

### Step 2: Environment Configuration

Create a `.env` file in the `honeypot_clean` directory with the following content:

```env
# RabbitMQ Configuration
RABBITMQ_URI=amqp://guest:guest@rabbitmq:5672/

# OpenAI Configuration (optional - for LLM integration)
OPEN_AI_SECRET_KEY=your_openai_key_here_optional
```

**Note:** The `OPEN_AI_SECRET_KEY` is optional. If not provided, LLM features will be disabled.

### Step 3: Start the Honeypot

```bash
# Build and start all services
docker-compose up -d

# Check if all containers are running
docker ps
```

You should see two containers running:
- `honeypot` - The main honeypot application
- `rabbitmq` - RabbitMQ message broker

### Step 4: Verify Services

```bash
# Check container status
docker ps

# Check honeypot logs
docker logs honeypot

# Check RabbitMQ logs
docker logs rabbitmq
```

## 🔧 Service Configuration

### Main Configuration (`beelzebub.yaml`)

```yaml
core:
  logging:
    debug: false
    debugReportCaller: false
    logDisableTimestamp: true
    logsPath: ./logs
  tracings:
    rabbit-mq:
      enabled: true
      uri: "amqp://guest:guest@rabbitmq:5672/"
  prometheus:
    path: /metrics
    port: ":2112"
  beelzebub-cloud:
    enabled: false
    uri: ""
    auth-token: ""
```

### Service Configurations

Each service in `configurations/services/` defines:
- **Protocol**: HTTP, SSH, MySQL, or TCP
- **Address**: Port to listen on
- **Commands**: Regex patterns and responses
- **Headers**: HTTP headers to send
- **Status codes**: HTTP response codes

## 🌐 Running Services and Ports

| Service | Protocol | Container Port | Host Port | Description |
|---------|----------|----------------|-----------|-------------|
| SSH Interactive | SSH | 22 | 2222 | Interactive SSH with command responses |
| SSH ChatGPT | SSH | 2222 | 2223 | SSH with ChatGPT integration |
| HTTP Apache 401 | HTTP | 8080 | 8080 | Returns 401 Unauthorized |
| HTTP Test | HTTP | 8081 | 8081 | Test service with custom response |
| HTTP WordPress | HTTP | 80 | 8082 | WordPress 6.0 emulation |
| MySQL | MySQL | 3306 | 3306 | MySQL 8.0.29 honeypot |
| TCP Generic | TCP | 3307 | 3307 | Generic TCP service |
| Prometheus | HTTP | 2112 | 2112 | Metrics endpoint |
| RabbitMQ | AMQP | 5672 | 5672 | Message broker |
| RabbitMQ Management | HTTP | 15672 | 15672 | Web management interface |

## 🧪 Testing Services

### SSH Services

```bash
# Test SSH on port 2222
ssh -p 2222 root@localhost
# Try passwords: root, toor, qwerty, Smoker666, 123456, jenkins, minecraft, sinus, alex, postgres, Ly123456

# Test SSH on port 2223
ssh -p 2223 root@localhost
```

**Available SSH Commands:**
- `ls` - Lists fake directories
- `pwd` - Shows current directory
- `uname -m` - Shows architecture
- `docker ps` - Shows fake containers
- `ps` - Shows fake processes

### HTTP Services

```bash
# Test HTTP 401 (port 8080)
curl -v http://localhost:8080/

# Test HTTP Test service (port 8081)
curl http://localhost:8081/

# Test WordPress (port 8082)
curl http://localhost:8082/
```

### MySQL Service

```bash
# Test MySQL connection
mysql -h localhost -P 3306 -u root -p
# Any password will work
```

### TCP Service

```bash
# Test TCP connection
telnet localhost 3307
# or
nc localhost 3307
```
or
start Everything
In PowerShell:
cd "C:\Users\Admin\Downloads\ai honeypot\honeypot_clean"
docker-compose down
docker-compose up --build -d
or in powershell
docker-compose down
docker-compose build
docker-compose up -d

. Check Container Status
docker ps
You should see both honeypot and rabbitmq containers Up (not restarting).

Check Honeypot Logs
docker logs honeypot
You should see service initialization messages, not fatal errors.

4. Test All Services

Ctrl+K to generate a command
http://localhost:8081/
This site can’t be reached
localhost refused to connect.
Try:

Checking the connection
Checking the proxy and the firewall
ERR_CONNECTION_REFUSED
http://localhost:8080/
This site can’t be reached
localhost refused to connect.
Try:

Checking the connection
Checking the proxy and the firewall
ERR_CONNECTION_REFUSED
http://localhost:8082/
This site can’t be reached
localhost refused to connect.
Try:

Checking the connection
Checking the proxy and the firewall
ERR_CONNECTION_REFUSED

fix everything nothing is running and workings

You are correct: none of your honeypot services are running or accessible.
Let’s do a full, step-by-step fix to get everything working.
1. Root Cause: Only RabbitMQ is Running
Your docker ps output shows only the RabbitMQ container is running and healthy.
The honeypot container is not running.
This is why all ports (SSH, HTTP, MySQL, etc.) are refusing connections.
2. Why is the Honeypot Container Not Running?
From your logs:
Apply to README.md
This means:
The honeypot container starts, tries to connect to RabbitMQ, but RabbitMQ is not ready yet.
The honeypot container crashes and never binds any ports.
3. The Solution: Wait for RabbitMQ to be Healthy Before Starting Honeypot
A. Add Healthcheck and depends_on to docker-compose.yml
Edit your honeypot_clean/docker-compose.yml so it looks like this:
1 hidden lines
1 hidden lines
3 hidden lines
1 hidden lines
B. Double-Check Your Config Files
honeypot_clean/configurations/beelzebub.yaml must exist and be valid.
All service YAMLs in honeypot_clean/configurations/services/ must be valid.
C. Rebuild and Restart Everything
In PowerShell:
Apply to README.md
Run
D. Check Container Status
Apply to README.md
Run
You should see both honeypot and rabbitmq containers Up (not restarting).
E. Check Honeypot Logs
Apply to README.md
Run
You should see service initialization messages, not fatal errors.
F. Test All Services
SSH:
ssh root@localhost -p 2222 (password: toor)
ssh admin@localhost -p 2223 (password: admin123)
HTTP:
Open http://localhost:8080, http://localhost:8081, http://localhost:8082
MySQL/TCP:
Test-NetConnection -ComputerName localhost -Port 3306
Test-NetConnection -ComputerName localhost -Port 3307
Prometheus:
http://localhost:2112/metrics
RabbitMQ:
http://localhost:15672 (user/pass: guest/guest)
If It Still Fails
Run:
Apply to README.md
Run
Copy the output here for further help.
Summary
Add depends_on and healthcheck to ensure honeypot waits for RabbitMQ.
Rebuild and restart everything.
Test all services as above.
Do these steps and your honeypot stack will work. If you hit any error, paste the logs and I’ll fix it for you!
fix everything n run the app and test all the services and queues and promestheus

You are very close! The main issue is that your honeypot container is still failing to connect to RabbitMQ at startup, even though RabbitMQ is healthy. This is a classic timing/race condition in Docker Compose setups.
Let's fix this step by step and ensure all services (SSH, HTTP, MySQL/TCP, Prometheus, RabbitMQ) are running and testable.
1. Ensure Correct docker-compose.yml
Your honeypot service must wait for RabbitMQ to be healthy.
Here is the correct snippet for your docker-compose.yml:
Make sure your .env file is not overriding RABBITMQ_URI with a wrong value.
2. Rebuild and Restart Everything
Apply to README.md
Run
Wait 30 seconds, then check:
Apply to README.md
Run
You should see both honeypot and rabbitmq containers Up (not restarting).
3. Check Honeypot Logs
Apply to README.md
Run
You should see service initialization messages, not fatal errors.
4. Test All Services
A. SSH
ssh-keygen -R [localhost]:2222
ssh-keygen -R [localhost]:2223
ssh root@localhost -p 2222
# password: toor
ssh admin@localhost -p 2223
# password: admin123


Ctrl+K to generate a command
http://localhost:8081/
This site can’t be reached
localhost refused to connect.
Try:

Checking the connection
Checking the proxy and the firewall
ERR_CONNECTION_REFUSED
http://localhost:8080/
This site can’t be reached
localhost refused to connect.
Try:

Checking the connection
Checking the proxy and the firewall
ERR_CONNECTION_REFUSED
http://localhost:8082/
This site can’t be reached
localhost refused to connect.
Try:

Checking the connection
Checking the proxy and the firewall
ERR_CONNECTION_REFUSED

fix everything nothing is running and workings

You are correct: none of your honeypot services are running or accessible.
Let’s do a full, step-by-step fix to get everything working.
1. Root Cause: Only RabbitMQ is Running
Your docker ps output shows only the RabbitMQ container is running and healthy.
The honeypot container is not running.
This is why all ports (SSH, HTTP, MySQL, etc.) are refusing connections.
2. Why is the Honeypot Container Not Running?
From your logs:
Apply to README.md
This means:
The honeypot container starts, tries to connect to RabbitMQ, but RabbitMQ is not ready yet.
The honeypot container crashes and never binds any ports.
3. The Solution: Wait for RabbitMQ to be Healthy Before Starting Honeypot
A. Add Healthcheck and depends_on to docker-compose.yml
Edit your honeypot_clean/docker-compose.yml so it looks like this:
1 hidden lines
1 hidden lines
3 hidden lines
1 hidden lines
B. Double-Check Your Config Files
honeypot_clean/configurations/beelzebub.yaml must exist and be valid.
All service YAMLs in honeypot_clean/configurations/services/ must be valid.
C. Rebuild and Restart Everything
In PowerShell:
Apply to README.md
Run
D. Check Container Status
Apply to README.md
Run
You should see both honeypot and rabbitmq containers Up (not restarting).
E. Check Honeypot Logs
Apply to README.md
Run
You should see service initialization messages, not fatal errors.
F. Test All Services
SSH:
ssh root@localhost -p 2222 (password: toor)
ssh admin@localhost -p 2223 (password: admin123)
HTTP:
Open http://localhost:8080, http://localhost:8081, http://localhost:8082
MySQL/TCP:
Test-NetConnection -ComputerName localhost -Port 3306
Test-NetConnection -ComputerName localhost -Port 3307
Prometheus:
http://localhost:2112/metrics
RabbitMQ:
http://localhost:15672 (user/pass: guest/guest)
If It Still Fails
Run:
Apply to README.md
Run
Copy the output here for further help.
Summary
Add depends_on and healthcheck to ensure honeypot waits for RabbitMQ.
Rebuild and restart everything.
Test all services as above.
Do these steps and your honeypot stack will work. If you hit any error, paste the logs and I’ll fix it for you!
fix everything n run the app and test all the services and queues and promestheus

You are very close! The main issue is that your honeypot container is still failing to connect to RabbitMQ at startup, even though RabbitMQ is healthy. This is a classic timing/race condition in Docker Compose setups.
Let's fix this step by step and ensure all services (SSH, HTTP, MySQL/TCP, Prometheus, RabbitMQ) are running and testable.
1. Ensure Correct docker-compose.yml
Your honeypot service must wait for RabbitMQ to be healthy.
Here is the correct snippet for your docker-compose.yml:
Make sure your .env file is not overriding RABBITMQ_URI with a wrong value.
2. Rebuild and Restart Everything
Apply to README.md
Run
Wait 30 seconds, then check:
Apply to README.md
Run
You should see both honeypot and rabbitmq containers Up (not restarting).
3. Check Honeypot Logs
Apply to README.md
Run
You should see service initialization messages, not fatal errors.
4. Test All Services
A. SSH
Apply to README.md
Run
B. HTTP
Open in your browser:
http://localhost:8080
http://localhost:8081
http://localhost:8082

C. MySQL/TCP
Test-NetConnection -ComputerName localhost -Port 3306
Test-NetConnection -ComputerName localhost -Port 3307
You should see TcpTestSucceeded : True.

D. Prometheus
Open http://localhost:2112/metrics in your browser.
E. RabbitMQ
Go to http://localhost:15672
Login: guest / guest
Check the event queue after making SSH/HTTP attempts.


## 📊 Monitoring and Logs

### RabbitMQ Management

Access the RabbitMQ management interface:
- **URL**: http://localhost:15672
- **Username**: guest
- **Password**: guest

**Check Queues:**
1. Go to "Queues" tab
2. Look for the "event" queue
3. Monitor message counts and rates

### Prometheus Metrics

Access Prometheus metrics:
- **URL**: http://localhost:2112/metrics

```bash
# Get metrics via curl
curl http://localhost:2112/metrics
```

### Container Logs

```bash
# Honeypot logs
docker logs honeypot

# RabbitMQ logs
docker logs rabbitmq

# Follow logs in real-time
docker logs -f honeypot
docker logs -f rabbitmq
```

## 🔍 Troubleshooting

### Common Issues

#### 1. Port Already in Use

```bash
# Check what's using a port
netstat -ano | findstr :5672

# Kill the process
taskkill /PID <PID> /F
```

#### 2. Docker Not Running

```bash
# Start Docker Desktop
# Wait for the whale icon to stop animating

# Verify Docker is running
docker version
```

#### 3. Container Not Starting

```bash
# Check container status
docker ps -a

# Check container logs
docker logs honeypot

# Restart containers
docker-compose restart
```

#### 4. Configuration File Not Found

Ensure `beelzebub.yaml` exists in `configurations/` directory:

```bash
# Check if file exists
ls -la configurations/beelzebub.yaml

# If missing, create it with the content shown above
```

#### 5. RabbitMQ Connection Issues

```bash
# Check RabbitMQ health
docker exec rabbitmq rabbitmq-diagnostics ping

# Check RabbitMQ status
docker exec rabbitmq rabbitmqctl status
```

### Service-Specific Issues

#### SSH Not Responding
```bash
# Check SSH service logs
docker logs honeypot | grep -i ssh

# Test SSH connection
ssh -v -p 2222 root@localhost
```

#### HTTP Services Not Working
```bash
# Check HTTP service logs
docker logs honeypot | grep -i http

# Test HTTP endpoints
curl -v http://localhost:8080/
curl -v http://localhost:8081/
curl -v http://localhost:8082/
```

#### MySQL Connection Issues
```bash
# Check MySQL service logs
docker logs honeypot | grep -i mysql

# Test MySQL connection
mysql -h localhost -P 3306 -u root -p
```

## 🛠️ Development

### Building from Source

```bash
# Build the application
go build -o main .

# Run tests
go test ./...

# Run with verbose output
go test ./... -v
```

### Using Makefile

```bash
# Start services
make beelzebub.start

# Stop services
make beelzebub.stop

# Run unit tests
make test.unit

# Run integration tests
make test.integration
```

### Adding New Services

1. Create a new YAML file in `configurations/services/`
2. Define the protocol, address, and commands
3. Restart the honeypot container

Example new service:
```yaml
apiVersion: "v1"
protocol: "http"
address: ":8083"
description: "Custom Service"
commands:
  - regex: ".*"
    handler: "Custom Response"
    headers:
      - "Content-Type: text/plain"
    statusCode: 200
```

## 🔒 Security Considerations

- This honeypot is designed to attract and log malicious activities
- Do not run on production systems without proper isolation
- Monitor logs regularly for suspicious activities
- Use strong passwords for any real services
- Consider using a separate network for honeypot deployment

## 📈 Performance Monitoring

### Key Metrics to Monitor

- **Connection rates** to each service
- **RabbitMQ queue depths**
- **Container resource usage**
- **Failed connection attempts**

### Resource Requirements

- **Minimum**: 2GB RAM, 2 CPU cores
- **Recommended**: 4GB RAM, 4 CPU cores
- **Storage**: 10GB for logs and data

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

For issues and questions:
1. Check the troubleshooting section above
2. Review container logs
3. Check RabbitMQ management interface
4. Verify all prerequisites are met

---

**Note**: This honeypot is for educational and research purposes. Always ensure you have proper authorization before deploying honeypots in any environment.
