Matrix Synapse + PostgreSQL Docker Installer
This bash script automates the deployment of a Matrix Synapse homeserver using Docker Compose and PostgreSQL. It streamlines the initial configuration, key generation, and performance tuning based on established community best practices.
Features
 * Automated Configuration: Handles the multi-step process of generating initial signing keys and then injecting them into a customized configuration file.
 * PostgreSQL Integration: Moves away from the default SQLite to a production-ready PostgreSQL 15 container.
 * Performance Tuning: Includes specific cache optimizations, federation rate-limiting, and memory usage targets.
 * Security: Automatically generates unique, 32-character hexadecimal database passwords during the setup process.
 * Media Management: Pre-configures a 500MB upload limit and 45-day retention policies for local and remote media.
Prerequisites
 * Docker: Ensure Docker and the Docker Compose plugin are installed.
 * OpenSSL: Required for secure password generation.
 * Root/Sudo Access: Necessary for directory creation and permission management (chown).
Usage
 * Prepare the script:
   Save the code as install_matrix.sh and make it executable:
   chmod +x install_matrix.sh

 * Run the installer:
   ./install_matrix.sh

 * Provide inputs:
   * Server Name: The domain or subdomain for your Matrix instance (e.g., https://www.google.com/search?q=matrix.example.com).
   * Install Directory: The location on your filesystem where data and configurations will reside.
Post-Installation Steps
1. Create User Accounts
Once the containers are healthy and the database migration completes, you can register users manually via the command line.
For an Admin User:
docker exec -it synapse register_new_matrix_user \
    -c /data/homeserver.yaml \
    -u <username> \
    -p <password> \
    --admin

For a Standard User:
If you want to create a regular user without administrative privileges, omit the --admin flag:
docker exec -it synapse register_new_matrix_user \
    -c /data/homeserver.yaml \
    -u <username> \
    -p <password>

2. Configure a Reverse Proxy
The Synapse container listens on port 8008 via HTTP. For production use, you must configure a reverse proxy (such as Nginx, Traefik, or Caddy) to provide SSL/TLS termination and port 443 mapping.
3. Maintenance
Logs can be monitored to ensure the federation and database connections are stable:
docker logs synapse -f

Directory Structure
The script creates the following structure within your chosen installation directory:
| Path | Description |
|---|---|
| docker-compose.yml | Defines the Synapse and Postgres services. |
| data/ | Synapse configuration, signing keys, and media store. |
| db_data/ | Persistent PostgreSQL database files. |
