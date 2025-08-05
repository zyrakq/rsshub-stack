# 📡 RSShub Service

A modular Docker Compose configuration system for RSShub RSS aggregator with support for multiple environments and extensions.

## 🏗️ Project Structure

```sh
src/rsshub/
├── components/                              # Source compose components
│   ├── base/                               # Base components
│   │   ├── docker-compose.yml              # Main RSShub service
│   │   ├── .env.example                    # Base environment variables
│   ├── environments/                       # Environment components
│   │   ├── devcontainer/
│   │   │   └── docker-compose.yml          # Development container environment
│   │   ├── forwarding/
│   │   │   └── docker-compose.yml          # Development with port forwarding
│   │   ├── letsencrypt/
│   │   │   ├── docker-compose.yml          # Let's Encrypt SSL
│   │   │   └── .env.example                # Let's Encrypt variables
│   │   └── step-ca/
│   │       ├── docker-compose.yml          # Step CA SSL
│   │       └── .env.example                # Step CA variables
│   └── extensions/                         # Extension components
│       ├── browserless/
│       │   ├── docker-compose.yml          # Browserless Chrome integration
│       │   └── .env.example                # Browserless variables
│       ├── guard/
│       │   ├── docker-compose.yml          # Authentication and access control
│       │   └── .env.example                # Authentication variables
│       └── redis/
│           ├── docker-compose.yml          # Redis caching
│           └── .env.example                # Redis variables
├── build/                        # Generated configurations (auto-generated)
│   ├── devcontainer/
│   │   ├── base/                 # Development container + base configuration
│   │   ├── browserless/          # Development container + browserless
│   │   ├── guard/                # Development container + authentication
│   │   ├── redis/                # Development container + redis
│   │   └── [combinations]/       # Various extension combinations
│   ├── forwarding/
│   │   ├── base/                 # Development + base configuration
│   │   ├── browserless/          # Development + browserless
│   │   ├── guard/                # Development + authentication
│   │   ├── redis/                # Development + redis
│   │   └── [combinations]/       # Various extension combinations
│   ├── letsencrypt/
│   │   ├── base/                 # Let's Encrypt + base configuration
│   │   ├── browserless/          # Let's Encrypt + browserless
│   │   ├── guard/                # Let's Encrypt + authentication
│   │   ├── redis/                # Let's Encrypt + redis
│   │   └── [combinations]/       # Various extension combinations
│   └── step-ca/
│       ├── base/                 # Step CA + base configuration
│       ├── browserless/          # Step CA + browserless
│       ├── guard/                # Step CA + authentication
│       ├── redis/                # Step CA + redis
│       └── [combinations]/       # Various extension combinations
├── build.sh                      # Build script
├── extensions.yml                # Extension compatibility configuration
└── README.md                     # This file
```

## 🚀 Quick Start

### 1. Build Configurations

Run the build script to generate all possible combinations:

```bash
./build.sh
```

This will create all combinations in the `build/` directory.

### 2. Choose Your Configuration

Navigate to the desired configuration directory:

```bash
# For development with port forwarding
cd build/forwarding/base/

# For development container environment
cd build/devcontainer/base/

# For production with Let's Encrypt SSL
cd build/letsencrypt/base/

# For production with Step CA SSL
cd build/step-ca/base/

# For development with authentication
cd build/forwarding/guard/

# For high-performance setup with Redis caching
cd build/forwarding/redis/

# For web scraping with Browserless Chrome
cd build/forwarding/browserless/
```

### 3. Configure Environment

Copy and edit the environment file:

```bash
cp .env.example .env
# Edit .env with your values
```

### 4. Deploy

Start the services:

```bash
docker-compose up -d
```

Access: `http://localhost:1200` (for forwarding mode)

## 🔧 Available Configurations

### Environments

- **devcontainer**: Development container environment for VS Code dev containers
- **forwarding**: Development environment with port forwarding (1200)
- **letsencrypt**: Production with Let's Encrypt SSL certificates
- **step-ca**: Production with Step CA SSL certificates

### Extensions

- **browserless**: Browserless Chrome integration for advanced web scraping
- **guard**: Authentication with access keys for secure RSS feeds
- **redis**: Redis caching for improved performance

### Generated Combinations

Each environment can be combined with any extension or combination of compatible extensions:

- `forwarding/base` - Development with port forwarding
- `forwarding/guard` - Development with authentication
- `forwarding/redis` - Development with Redis caching
- `forwarding/browserless` - Development with Browserless Chrome
- `forwarding/browserless+guard` - Development with Browserless + authentication
- `forwarding/browserless+redis` - Development with Browserless + Redis
- `forwarding/guard+redis` - Development with authentication + Redis
- `forwarding/browserless+guard+redis` - Development with all extensions
- `letsencrypt/base` - Production with Let's Encrypt SSL
- `letsencrypt/guard` - Production with Let's Encrypt + authentication
- `step-ca/base` - Production with Step CA SSL
- `step-ca/guard` - Production with Step CA + authentication

## 🔧 Environment Variables

### Base Configuration

- `NODE_ENV`: Node.js environment (default: production)
- `CACHE_TYPE`: Cache type (default: none, options: memory, redis)
- `COMPOSE_PROJECT_NAME`: Project name for Docker Compose (default: rsshub)

### Let's Encrypt Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 1200)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Step CA Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 1200)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Guard Configuration (Authentication)

- `ACCESS_KEY`: Access key for RSS feed access (default: rsshub)

### Redis Configuration (Caching)

- `CACHE_TYPE`: Set to redis for Redis caching
- `REDIS_URL`: Redis connection URL (default: redis://rsshub-redis:6379/)

### Browserless Configuration (Web Scraping)

- `PUPPETEER_WS_ENDPOINT`: Browserless WebSocket endpoint (default: ws://rsshub-browserless:3000)

## 📡 Using RSShub

### Accessing RSS Feeds

```bash
# Without authentication (base configurations)
curl http://localhost:1200/github/issue/DIYgod/RSSHub

# With authentication (guard configurations)
curl http://localhost:1200/github/issue/DIYgod/RSSHub?key=YOUR_ACCESS_KEY
```

### Popular RSS Routes

```bash
# GitHub repository releases
http://localhost:1200/github/release/DIYgod/RSSHub

# Twitter user timeline
http://localhost:1200/twitter/user/DIYgod

# YouTube channel
http://localhost:1200/youtube/user/@DIYgod

# Reddit subreddit
http://localhost:1200/reddit/r/rss
```

## 🛠️ Development

### Adding New Environments

1. Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations

### Adding New Extensions

1. Create directory in `components/extensions/` with `docker-compose.yml` and optional `.env.example` file
2. Update `extensions.yml` if needed for compatibility rules
3. Run `./build.sh` to generate new combinations

### File Naming Convention

All component files follow the standard Docker Compose naming convention (`docker-compose.yml`) for:

- **VS Code compatibility**: Full support for Docker Compose language features and IntelliSense
- **IDE integration**: Proper syntax highlighting and validation in all major editors
- **Tool compatibility**: Works with Docker Compose plugins and extensions
- **Standard compliance**: Follows official Docker Compose file naming patterns

### Modifying Existing Components

1. Edit the component files in `components/`
2. Run `./build.sh` to regenerate configurations
3. The `build/` directory will be completely recreated

## 🌐 Networks

- **Development**: `rsshub-network` (internal)
- **Let's Encrypt**: `letsencrypt-network` (external)
- **Step CA**: `step-ca-network` (external)

## 🔒 Security

⚠️ **Production Checklist:**

- Change default access key
- Configure firewall rules
- Regular security updates
- Use HTTPS in production environments

## 🆘 Troubleshooting

**Build Issues:**

- Ensure `yq` is installed: <https://github.com/mikefarah/yq#install>
- Check component file syntax
- Verify all required files exist

**RSS Feed Issues:**

- Check access key correctness (for guard configurations)
- Ensure service is running and accessible
- Review container logs: `docker logs rsshub`

**Performance Issues:**

- Consider using Redis caching for high-traffic scenarios
- Use Browserless for sites requiring JavaScript rendering
- Monitor container resource usage

**SSL Issues:**

- **Let's Encrypt**: Verify domain DNS and letsencrypt-manager
- **Step CA**: Check step-ca-manager and virtual network config

## 📝 Notes

- The `build/` directory is automatically generated and should not be edited manually
- Environment variables in generated files use `$VARIABLE_NAME` format for proper interpolation
- Each generated configuration includes a complete `docker-compose.yml` and `.env.example`
- Missing `.env.*` files for components are handled gracefully by the build script
- Extension combinations are defined in `extensions.yml` for compatibility management

## 🔄 Extension Compatibility

The `extensions.yml` file defines:

- **Groups**: Logical groupings of extensions (browsers, caches, guard)
- **Conflicts**: Extensions that cannot be used together
- **Combinations**: Pre-defined extension combinations
- **Naming**: How combination names are generated

This ensures only compatible extensions are combined during the build process.
