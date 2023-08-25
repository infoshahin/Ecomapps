---

# PHP Application with MySQL using Docker.

This repository contains the necessary Dockerfiles [(here)](https://github.com/infoshahin/Ecomapps/blob/master/Dockerfile) to set up a PHP application with a MySQL database. We use a specific MySQL version, `5.6`, and the services can be orchestrated with Docker Compose (though Docker Compose instructions are not provided here).

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


# GitHub Action for CI/CD

I have implemented a continuous integration and continuous deployment (CI/CD) pipeline for our PHP application using GitHub Actions. The entire workflow is defined in the `.github/workflows/ci-cd.yml` file, which you can view [here](https://github.com/infoshahin/Ecomapps/blob/master/.github/workflows/ci-cd.yml).

#### Workflow Triggers

- **Trigger on Push to Main Branch**: This workflow is triggered on every push to the main branch, ensuring that the latest code changes are automatically tested, built, and deployed.

#### Key Workflow Steps

1. **Builds the Docker Image for the PHP Application**: Utilizes Docker Buildx to create a Docker image of the PHP application, encapsulating all required dependencies and configurations.

2. **Sets up Required Services**: Starts the services defined in `docker-compose.yml`, including the PHP application and MySQL 5.6, ensuring that they are up and running for further steps. `docker-compose.yml` file, you can view [here](https://github.com/infoshahin/Ecomapps/blob/master/docker-compose.yml).

3. **Runs a Simple Test**: Executes a simple test using a curl command to ensure that the application is responding as expected. This is an essential verification step before proceeding to deployment. (curl command to ensure the application responds)

4. **Pushes the Docker Image**: If the tests pass, the built Docker image for the PHP application is pushed to a Docker registry, such as Docker Hub. This step makes the image available for deployment to different environments.

5. **Deploys the Docker Compose Stack**: If the tests pass, the Docker Compose stack is deployed to a server. In our case, I am using an AWS EC2 instance, and the SSH command used for connecting is:

   ```
   ssh -i "awsbookingdei.pem" ubuntu@ec2-18-141-188-60.ap-southeast-1.compute.amazonaws.com
   ```

#### Conclusion

This GitHub Action for CI/CD ensures a streamlined and automated process for building, testing, and deploying our PHP application. By leveraging Docker and GitHub Actions, I can easily manage and scale our application across different stages and environments. Finally, User Signup and Sign in functionalities are working as expected.

# Screenshots
![Signup]
![1](https://github.com/infoshahin/Ecomapps/assets/8981516/ee714e9f-0f68-4e3a-b71b-87a5ba97c285)

![Sign In](file:///C:/Users/habib/Downloads/1.1.png)
![1 1](https://github.com/infoshahin/Ecomapps/assets/8981516/8321dea7-ff8a-4993-b9e9-6983730d0e9c)

![After Logged in](file:///C:/Users/habib/Downloads/2.png)
![2](https://github.com/infoshahin/Ecomapps/assets/8981516/2169604b-6c4d-48bf-80f3-e0a4b8616ff3)


---
