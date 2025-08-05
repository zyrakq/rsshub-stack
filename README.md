# 🚀 RSShub Stack

A comprehensive Docker stack for running RSShub RSS aggregator with certificate management solutions.

A lightweight RSS aggregator and feed generator implementation. Perfect for hosting RSS feeds with Docker support and various deployment configurations.

## 🧩 Components

### 🔐 SSL Automation

#### [🔒 Let's Encrypt Manager](src/ssl-automation/letsencrypt-manager)

Automatic SSL certificate management from Let's Encrypt for production deployments. Provides seamless HTTPS integration for Docker containers using nginx-proxy and acme-companion.
[Learn more about Let's Encrypt Manager configuration](src/ssl-automation/letsencrypt-manager/README.md).

#### [🏠 Step CA Manager](src/ssl-automation/step-ca-manager)

Local domain stack with trusted self-signed certificates for virtual network deployments. Includes private CA management and local DNS resolution for development environments.
[Learn more about Step CA Manager configuration](src/ssl-automation/step-ca-manager/README.md).

## 🌐 Services

### 📡 RSShub

**Location:** [`src/rsshub/`](src/rsshub/)

A modular Docker Compose configuration system for RSShub RSS aggregator with support for multiple environments and extensions including Redis caching, Browserless Chrome integration, and authentication.

## 🎯 Use Cases

- **🌐 Production Deployment**: Use RSShub + Let's Encrypt Manager for public-facing RSS aggregator servers
- **🏠 Internal Networks**: Use RSShub + Step CA Manager for private/development environments
- **🔧 Development**: Use RSShub standalone for local development
- **⚡ High Performance**: Use RSShub + Redis for caching and improved performance
- **🔒 Secure Access**: Use RSShub + Guard for authentication and access control
- **🌐 Web Scraping**: Use RSShub + Browserless for advanced web scraping capabilities

## 🚀 Quick Start

Each component has its own README with detailed setup instructions. Choose the certificate management solution that fits your deployment scenario.

### Basic Development Setup

```bash
cd src/rsshub/
./build.sh
cd build/forwarding/base/
cp .env.example .env
docker-compose up -d
```

Access: `http://localhost:1200`

### Production Setup with SSL

```bash
cd src/rsshub/
./build.sh
cd build/letsencrypt/base/
cp .env.example .env
# Edit .env with your domain and email
docker-compose up -d
```

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)
