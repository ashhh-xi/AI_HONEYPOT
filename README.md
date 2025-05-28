# Honeypot Framework

[![Go Report Card](https://goreportcard.com/badge/github.com/sudarshantk/honeypot)](https://goreportcard.com/report/github.com/sudarshantk/honeypot)
[![Go Reference](https://pkg.go.dev/badge/github.com/sudarshantk/honeypot.svg)](https://pkg.go.dev/github.com/sudarshantk/honeypot)

A powerful honeypot framework that allows you to create and manage honeypots for various protocols including SSH, HTTP, and TCP. This framework helps you monitor and analyze potential security threats by simulating vulnerable services.

## Overview

This is an advanced honeypot framework designed to provide a highly secure environment for detecting and analyzing cyber attacks. It offers a low code approach for easy implementation and uses AI to mimic the behavior of a high-interaction honeypot.

## LLM Honeypot

[![asciicast](https://asciinema.org/a/665295.svg)](https://asciinema.org/a/665295)

## Telegram Bot for Real-Time Attacks

Stay updated on real-time attacks by joining our dedicated Telegram channel: [Telegram Channel](https://t.me/beelzebubhoneypot)

## Examples

To better understand the capabilities of this framework, you can explore our example repository: [sudarshantk/honeypot-example](https://github.com/sudarshantk/honeypot-example)

## Quick Start

### Using Docker Compose

1. Build the Docker images:
   ```bash
   $ docker-compose build
   ```

2. Start the framework in detached mode:
   ```bash
   $ docker-compose up -d
   ```

### Using Go Compiler

1. Download the necessary Go modules:
   ```bash
   $ go mod download
   ```

2. Build the executable:
   ```bash
   $ go build
   ```

3. Run the framework:
   ```bash
   $ ./main
   ```

### Deploy on kubernetes cluster using helm

1. Install helm

2. Deploy the framework:
   ```bash
   $ helm install honeypot ./honeypot-chart
   ```

3. Next release:
   ```bash
   $ helm upgrade honeypot ./honeypot-chart
   ```

## Testing

### Unit Tests
```bash
$ make test.unit
```

### Integration Tests
```bash
$ make test.dependencies.start
$ make test.integration
$ make test.dependencies.down
```

## Configuration

The framework allows easy configuration for different services and ports. Create a new file for each service/port within the `/configurations/services` directory.

To execute with your custom path:
```bash
$ ./main --confCore ./configurations/config.yaml --confServices ./configurations/services/
```

## Key Features

- Support for OpenAI integration
- SSH Honeypot
- HTTP Honeypot
- TCP Honeypot
- Prometheus openmetrics integration
- Docker integration
- RabbitMQ integration
- Kubernetes support

## Example Configuration

Beelzebub allows easy configuration for different services and ports. Simply create a new file for each service/port within the `/configurations/services` directory.

To execute Beelzebub with your custom path, use the following command:

```bash
$ ./beelzebub --confCore ./configurations/beelzebub.yaml --confServices ./configurations/services/
```

Here are some example configurations for different honeypot scenarios:

#### Example HTTP Honeypot on Port 80

###### http-80.yaml

```yaml
apiVersion: "v1"
protocol: "http"
address: ":80"
description: "Wordpress 6.0"
commands:
  - regex: "^(/index.php|/index.html|/)$"
    handler:
      <html>
        <header>
          <title>Wordpress 6 test page</title>
        </header>
        <body>
          <h1>Hello from Wordpress</h1>
        </body>
      </html>
    headers:
      - "Content-Type: text/html"
      - "Server: Apache/2.4.53 (Debian)"
      - "X-Powered-By: PHP/7.4.29"
    statusCode: 200
  - regex: "^(/wp-login.php|/wp-admin)$"
    handler:
      <html>
        <header>
          <title>Wordpress 6 test page</title>
        </header>
        <body>
          <form action="" method="post">
            <label for="uname"><b>Username</b></label>
            <input type="text" placeholder="Enter Username" name="uname" required>

            <label for="psw"><b>Password</b></label>
            <input type="password" placeholder="Enter Password" name="psw" required>

            <button type="submit">Login</button>
          </form>
        </body>
      </html>
    headers:
      - "Content-Type: text/html"
      - "Server: Apache/2.4.53 (Debian)"
      - "X-Powered-By: PHP/7.4.29"
    statusCode: 200
  - regex: "^.*$"
    handler:
      <html>
        <header>
          <title>404</title>
        </header>
        <body>
          <h1>Not found!</h1>
        </body>
      </html>
    headers:
      - "Content-Type: text/html"
      - "Server: Apache/2.4.53 (Debian)"
      - "X-Powered-By: PHP/7.4.29"
    statusCode: 404
```

#### Example HTTP Honeypot on Port 8080

###### http-8080.yaml

```yaml
apiVersion: "v1"
protocol: "http"
address: ":8080"
description: "Apache 401"
commands:
  - regex: ".*"
    handler: "Unauthorized"
    headers:
      - "www-Authenticate: Basic"
      - "server: Apache"
    statusCode: 401
```

#### Example SSH Honeypot

###### LLM Honeypots

Follow a SSH LLM Honeypot using OpenAI as provider LLM:

```yaml
apiVersion: "v1"
protocol: "ssh"
address: ":2222"
description: "SSH interactive OpenAI  GPT-4"
commands:
  - regex: "^(.+)$"
    plugin: "LLMHoneypot"
serverVersion: "OpenSSH"
serverName: "ubuntu"
passwordRegex: "^(root|qwerty|Smoker666|123456|jenkins|minecraft|sinus|alex|postgres|Ly123456)$"
deadlineTimeoutSeconds: 60
plugin:
   llmProvider: "openai"
   llmModel: "gpt-4o" #Models https://platform.openai.com/docs/models
   openAISecretKey: "sk-proj-123456"
```

Examples with local Ollama instance using model codellama:7b:

```yaml
apiVersion: "v1"
protocol: "ssh"
address: ":2222"
description: "SSH Ollama Llama3"
commands:
  - regex: "^(.+)$"
    plugin: "LLMHoneypot"
serverVersion: "OpenSSH"
serverName: "ubuntu"
passwordRegex: "^(root|qwerty|Smoker666|123456|jenkins|minecraft|sinus|alex|postgres|Ly123456)$"
deadlineTimeoutSeconds: 60
plugin:
   llmProvider: "ollama"
   llmModel: "codellama:7b" #Models https://ollama.com/search
   host: "http://example.com/api/chat" #default http://localhost:11434/api/chat
```
Example with custom prompt:

```yaml
apiVersion: "v1"
protocol: "ssh"
address: ":2222"
description: "SSH interactive OpenAI  GPT-4"
commands:
  - regex: "^(.+)$"
    plugin: "LLMHoneypot"
serverVersion: "OpenSSH"
serverName: "ubuntu"
passwordRegex: "^(root|qwerty|Smoker666|123456|jenkins|minecraft|sinus|alex|postgres|Ly123456)$"
deadlineTimeoutSeconds: 60
plugin:
   llmProvider: "openai"
   llmModel: "gpt-4o"
   openAISecretKey: "sk-proj-123456"
   prompt: "You will act as an Ubuntu Linux terminal. The user will type commands, and you are to reply with what the terminal should show. Your responses must be contained within a single code block."
```

###### SSH Honeypot on Port 22

###### ssh-22.yaml

```yaml
apiVersion: "v1"
protocol: "ssh"
address: ":22"


description: "SSH interactive"
commands:
  - regex: "^ls$"
    handler: "Documents Images Desktop Downloads .m2 .kube .ssh .docker"
  - regex: "^pwd$"
    handler: "/home/"
  - regex: "^uname -m$"
    handler: "x86_64"
  - regex: "^docker ps$"
    handler: "CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES"
  - regex: "^docker .*$"
    handler: "Error response from daemon: dial unix docker.raw.sock: connect: connection refused"
  - regex: "^uname$"
    handler: "Linux"
  - regex: "^ps$"
    handler: "PID TTY TIME CMD\n21642 ttys000 0:00.07 /bin/dockerd"
  - regex: "^(.+)$"
    handler: "command not found"
serverVersion: "OpenSSH"
serverName: "ubuntu"
passwordRegex: "^(root|qwerty|Smoker666)$"
deadlineTimeoutSeconds: 60
```

## Roadmap

Our future plans for Beelzebub include developing it into a robust PaaS platform.

## Contributing

The Beelzebub team welcomes contributions and project participation. Whether you want to report bugs, contribute new features, or have any questions, please refer to our [Contributor Guide](CONTRIBUTING.md) for detailed information. We encourage all participants and maintainers to adhere to our [Code of Conduct](CODE_OF_CONDUCT.md) and foster a supportive and respectful community.

Happy hacking!

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Supported by
[![JetBrains logo.](https://resources.jetbrains.com/storage/products/company/brand/logos/jetbrains.svg)](https://jb.gg/OpenSourceSupport)

![gitbook logo](https://i.postimg.cc/VNQh5hnk/gitbook.png)