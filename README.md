# **dock**: Docker Helper Script

**Table of Contents**
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
    - [Certificate Commands](#certificate-commands)
    - [docker-compose Commands](#docker-compose-commands)
    - [SSH Commands](#ssh-commands)
- [Examples](#examples)
- [Tips](#tips)
    - [MySQL Grants](#mysql-grants)
    - [SSL Certificates for HTTPS](#ssl-certificates-for-https)
- [Contributing](#contributing)
- [License](#license)

## Introduction
`dock` is a Docker Helper Script that simplifies the management of Docker containers and provides convenience functions 
for working with Docker Compose, SSL certificates, and SSH access within containers.

## Prerequisites
Before using Dock, ensure you have the following prerequisites installed on your system:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [mkcert](https://github.com/FiloSottile/mkcert)

## Installation
Follow these steps to install and configure Dock on your system:

1. Clone the Dock repository to your local machine:
   ```shell
   git clone https://github.com/puncoz-official/dock.git
   ```

2. Copy the `env.example` file and update it with your desired configuration:
   ```shell
   cp env.example .env
   # Edit the .env file with your configurations
   ```

3. _[Optional]_ For convenience, add the `./dock` script to your system's PATH. On macOS or Linux, you can do this by adding the 
following line to your shell profile (e.g., `~/.bashrc`, `~/.zshrc`):
   ```shell
   export PATH="$PATH:/path/to/dock"
   ```

   On Windows, you can add the directory containing `dock` to your system's environment PATH.

4. Apply SSL certificates by running the following command:
   ```shell
   dock certs:apply
   ```

5. Start the Docker Compose application by running:
   ```shell
   # to start all services
   dock up
   
   # to start specific services 
   dock up <service1-name> <service2-name>
   ```

## Usage
Dock provides several commands to manage your Docker environment.

### Certificate Commands
Generate and manage SSL certificates for your domains.

- **Generate Certificates for Domains:**
  ```shell
  dock certs [-n cert_name] [-d domain_name_1] [-d domain_name_2] ...
  ```

  Example:
  ```shell
  dock certs -n my_app -d "my-app.localhost" -d "*.my-app.localhost"
  ```

- **Apply Generated Certificates:**
  ```shell
  dock certs:apply
  ```

### docker-compose Commands
Manage Docker Compose services.

- **Start the Application (Runs in the Background):**
  ```shell
  dock up
  ```

- **Stop the Application:**
  ```shell
  dock down
  ```

- **Supports All Docker-Compose Commands:**
  ```shell
  dock [commands]
  
  # eg:
  dock ps
  ```

### SSH Commands
Access shell sessions within Docker containers.

- **Start a Shell Session within the Application Container (as Root User):**
  ```shell
  dock ssh <service-name>
  ```

- **Start a Shell Session within the Application Container (as a User Supplied):**
  ```shell
  dock ssh user@<service-name>
  ```

## Examples
Here are some common usage examples of Dock:

- Generate SSL certificates for a domain:
  ```shell
  dock certs -n my_app -d "my-app.localhost" -d "*.my-app.localhost"
  ```

- Apply SSL certificates:
  ```shell
  dock certs:apply
  ```

- Start the Docker Compose application:
  ```shell
  dock up
  ```

- Stop the Docker Compose application:
  ```shell
  dock down
  ```

- Access a shell session within the application container as the root user:
  ```shell
  dock ssh <service-name>
  ```

- Access a shell session within the application container as a specific user:
  ```shell
  dock ssh user@<service-name>
  ```

## Tips
### MySQL Grants
If you encounter an "Access denied for user 'root'@'localhost'" issue with MySQL 5.7, follow these steps before 
creating a database:

Run the following command (replace `'password'` with your desired password) before creating the database. **If the 
database is already created, drop the database and recreate it after running this command:**

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password';
```

### SSL Certificates for HTTPS
To generate SSL certificates for HTTPS, follow these steps:

1. Install [mkcert](https://github.com/FiloSottile/mkcert):
   ```shell
   # If it's the first install of mkcert, run
   mkcert -install
   ```

2. Generate a certificate for your domain and store it in the `certs` directory:
   ```shell
   mkcert -cert-file ./data/certs/local-cert.pem -key-file ./data/certs/local-key.pem "localhost" "subdomain.localhost"
   ```

   This will create two files: `local-cert.pem` and `local-key.pem` in the `certs` directory, which can be used for 
configuring HTTPS in your application.

## Contributing
Contributions to `dock` are welcome! If you have any suggestions, bug reports, or feature requests, please create an 
issue or submit a pull request on the [GitHub repository](https://github.com/puncoz-official/dock).

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
