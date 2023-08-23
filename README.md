---

# PHP Application with MySQL using Docker.

This repository contains the necessary Dockerfiles to set up a PHP application with a MySQL database. We use a specific MySQL version, `5.6`, and the services can be orchestrated with Docker Compose (though Docker Compose instructions are not provided here).

## Setup & Running the Application

1. **Building the PHP Application Docker Image**:
   
   Build the Docker image for the PHP application using the provided Dockerfile.
   
   ```bash
   docker build -t ecomimage5 .
   ```

2. **Pulling the MySQL Image**:

   We use the specific MySQL version `5.6` for our database.

   ```bash
   docker pull mysql:5.6
   ```

3. **Creating a Docker Network**:

   We'll create a Docker network `ecomnet` to allow the containers to communicate with each other.

   ```bash
   docker network create ecomnet
   ```

4. **Running the MySQL Container**:

   Start a MySQL container with the root password set to `password123` and initialize a database named `mshop`.

   ```bash
   docker run -d -p 3306:3306 --name db --network ecomnet -e MYSQL_ROOT_PASSWORD=password123 -e MYSQL_DATABASE=mshop mysql:5.6
   ```

5. **Running the PHP Application Container**:

   Start the PHP application container, attaching it to the `ecomnet` network.

   ```bash
   docker run -d -p 5600:80 --name ecomapp --network ecomnet -e SERVER_NAME=localhost ecomimage5
   ```

   After running this command, your PHP application will be available at [http://localhost:5600](http://localhost:5600).

6. **Accessing the MySQL Container's Shell**:

   If you need to access the MySQL container's shell, you can use the following command:

   ```bash
   docker exec -it db bash
   ```

   Inside the container:

   1. Log in to the MySQL CLI using the root user.
      ```bash
      mysql -u root -p
      ```

      When prompted, enter the password `password123`.

   2. Switch to the `mshop` database.
      ```sql
      use mshop;
      ```


# Using Docker Compose YAML file

 ```bash
      docker compose up -d
 ```
This command will build, create, start, and attach to containers for a service.

---
