# Bedrock WordPress Development Environment with Lando

This repository contains a development environment setup for WordPress using Roots Bedrock architecture with Lando as the local development environment.

## Overview

This setup provides a complete LEMP (Linux, Nginx, MySQL, PHP) stack for local WordPress development using Bedrock, with the following features:

- PHP 8.2
- MySQL 8.0
- Nginx web server
- Automatic Bedrock installation
- Environment variables configuration
- REST API configuration
- phpMyAdmin for database management
- MailHog for email testing
- WP-CLI for WordPress management

## Prerequisites

- [Lando](https://docs.lando.dev/getting-started/installation.html)
- [Composer](https://getcomposer.org/download/)
- Git

## Getting Started

1. Clone this repository
2. Navigate to the project directory
3. Run `lando start`

The setup will automatically:
- Install Bedrock if not already present
- Configure the `.env` file with database connection details
- Install WordPress and set up an admin user

## Configuration Files

### .lando.yml

The main configuration file that defines the development environment:

- LEMP stack configuration
- Service definitions (database, phpMyAdmin, MailHog)
- Custom tooling for Composer and WP-CLI
- Event hooks for setup automation
- Environment variable configuration

### nginx.conf

Custom Nginx configuration with important settings:

- Server definition for the WordPress site
- PHP handling
- **REST API configuration** - Includes CORS headers for the WP REST API at `/wp-json/` to allow cross-origin requests, which is essential for headless WordPress setups.

```nginx
location /wp-json/ {
    if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type, X-WP-Nonce';
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain charset=UTF-8';
        add_header 'Content-Length' 0;
        return 204;
    }
    try_files $uri $uri/ /index.php?$args;
}
```

### php.ini

Custom PHP configuration optimized for WordPress development:

- Error reporting settings
- Memory limits increased to 256MB
- Execution time extended to 300 seconds
- Upload limits increased to 64MB
- URL fopen enabled

## URLs

- **WordPress site**: https://wp-starter.local
- **phpMyAdmin**: https://pma.wp-starter.local

## Default WordPress Credentials

- **Username**: admin
- **Password**: admin
- **Email**: admin@wp-starter.local

## Common Commands

```bash
# Start the environment
lando start

# Stop the environment
lando stop

# Access WP-CLI
lando wp [command]

# Access Composer
lando composer [command]

# SSH into the application container
lando ssh -s appserver

# View logs
lando logs

# Rebuild the environment
lando rebuild
```

## Important Notes

1. This is a **development environment only** and should not be used for production.
2. The `.env` file is automatically configured with database credentials from Lando.
3. Custom fields in Bedrock architecture can be managed through [Advanced Custom Fields](https://www.advancedcustomfields.com/) or similar plugins.
4. The REST API is configured to allow cross-origin requests, which is useful for headless WordPress setups.

## Customization

- Modify `.lando.yml` to add additional services or change configuration
- Update `nginx.conf` for web server customization
- Adjust `php.ini` for PHP settings

## Troubleshooting

- If the site doesn't load, check that the domain is properly set up in your hosts file
- For database connection issues, verify the credentials in the `.env` file
- If REST API requests fail, ensure the proper headers are set in `nginx.conf`

## License

[MIT](https://opensource.org/licenses/MIT)