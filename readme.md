# Jenkins Pipeline for Web Page Application (Postgresql-Nodejs-React) deployed on AWS-EC2 with Ansible and Docker

## Description

This project aims to create a Jenkins pipeline to deploy web-page written Nodejs and React Frameworks on AWS Cloud Infrastructure using Ansible. Building infrastructure process is managing with control node utilizing Ansible. This infrastructure has one jenkins server (`Amazon Linux 2 AMI`) as ansible control node and 3 EC2s as worker node. These EC2s will be launched on AWS console. Web-page has 3 main components which are postgresql, nodejs, and react. Each component is serving in Docker container on EC2s dedicated for them. Postgresql is serving as Database of web-page. Nodejs controls backend part of web-side and react controls frontend side of web-page.

## Project Details

![jenkins](https://user-images.githubusercontent.com/114299019/221368009-389f4ea5-fe84-4677-8a74-d2e76eadcf3d.png)

- Web-page allows users to collect their infos. Registration data should be kept in separate PostgreSQL database located in one of EC2s. Nodejs framework controls backend and serves on port 5000, it is also connected to the PostgreSQL database on port 5432. React framework controls the frontend and it is also connected to the Nodejs server on port 5000. React server broadcasts web-page on port 3000.

- The Web Application will be deployed using Nodejs and React framework.

- The Web Application should be accessible via web browser from anywhere on port 3000.

- EC2s and their security groups should be created on AWS with Terraform.

- The rest of the process has to be controlled with control node which is connected SSH port.

- Codes should be pulled from the Repo into the Jenkins server and sent them to the EC2s from there with Ansible.

- Postgresql, Nodejs and React parts has to be placed in docker container.

- Launch an EC2 for each postgresql, nodejs and react docker container. In addition, write one playbook for this project to install and configure docker and create containers in each instances.

- Use AWS ECR as image repository, to create Docker Containers with 3 managed nodes (postgresql, nodejs and react EC2 instances).

- All process has to be controlled into the `jenkins server` as control node.

- Dynamic inventory (`inventory_aws_ec2.yml`) has to be used for inventory file.

- Ansible config file (`ansible.cfg`) has to be placed in jenkins server.

- Docker should be installed in all worker nodes using ansible.

- Docker images should be build in jenkins server and pushed to the AWS ECR.

- For PostgreSQL worker node

    - Docker image should be created for PostgreSQL container with `dockerfile-postgresql` and `init.sql` file should be placed under necessary folder.

    - Docker images should be build in jenkins server from `dockerfile-postgresql` and pushed to the AWS ECR.

    - Create PostgreSQL container in the worker node. Do not forget to set password as environmental variable.

    - This instance's security group should accept traffic from PostgreSQL's dedicated port from Nodejs EC2 and port 22 from anywhere.

    - To keep database's data, volume has to be created with docker container and necessary file(s) should be kept under this file.

- For Nodejs worker node

	- Make sure to correct or create `.env` file under `server` folder based on PostgreSQL environmental variables. To automate taking private ip of postgresql instance you can use linux `envsubst` command with env-template.

	- Docker image should be created for nodejs container with dockerfile-nodejs and `server` files should be placed under necessary folder. This file will use for docker image. You don't need any extra file for creating Nodejs image.

	- Docker image should be built for Nodejs container in jenkins server from `dockerfile-nodejs` and pushed to the AWS ECR.

	- Create Nodejs container in nodejs instance with ansible and publish it on port 5000.

	- This instance's security group should accept traffic from 5000, 22 dedicated port from anywhere.

- For React worker node

	- Make sure to correct `.env` file under `client` folder based on Nodejs environmental variables. To automate taking public ip of nodejs instance, you can use linux `envsubst` command with env-template.

	- Docker image should be created for react container with dockerfile-react and `client` files should be placed under necessary folder. This file will be used for docker image. You don't need any extra file for creating react image.

	- Docker image should be built for React container in jenkins server from `dockerfile-react` and pushed to the AWS ECR.

	- Create React container and publish it on port 3000.

	- This instance's security group should accept traffic from 3000, and 80 dedicated port from anywhere.

- To use the AWS ECR as image repository;

	- Enable the instances with IAM Role allowing them to work with ECR repos using the instance profile.

	- Install AWS CLI `Version 2` on worker node instances to use `aws ecr` commands.

- To create a Jenkins Pipeline, you need to launch a Jenkins Server with security group allowing SSH (port 22) and HTTP (ports 80, 8080) connections. For this purpose, you can use pre-configured [*Terraform configuration file for Jenkins Server enabled with Git, Docker, Terraform, Ansible and also configured to work with AWS ECR using IAM role*](./jenkins_server/install-jenkins.tf).

- Create a Jenkins Pipeline with following configuration;

  - Build the infrastructure for the app on EC2 instances using Terraform configuration file.

  - Create an image repository on ECR for the app.

  - Build the application Docker image and push it to the same ECR repository with different tags.

  - Deploy the application on AWS EC2's with ansible.

  - Make a failure stage and ensure to destroy infrastructure, ECR repo and docker images when the pipeline failed.


## Project Skeleton

```bash
Jenkins-project (folder)
|
|----Readme.md                  # Definition of the project
|----Jenkins_Project.png        # Project diagram
|----dockerfile-postgresql
|----dockerfile-nodejs
|----dockerfile-react
|----main.tf                    # For managed nodes
|----variables.tf
|----outputs.tf
|----Jenkinsfile                # Jenkins pipeline file
|----install-jenkins.tf         # install jenkins server
|----nodejs (folder)            # Nodejs folders and files
|----react (folder)             # React folders and files
|----postgresql (folder)        # Postgresql file
|----ansible.cfg                # Ansible configuration file
|----inventory_aws_ec2.yml      # Ansible inventory file
|----node-env-template          # env template to take postgresql node private ip)
|----react-env-template         # env template to take nodejs node private ip)

```

#
  
 ## Conclusion
This project provides a straightforward and automated solution for deploying a web application built with PostgreSQL, Node.js, and React frameworks on AWS infrastructure. The use of Ansible for infrastructure orchestration and Docker for containerization streamlines the deployment process, while the Jenkins pipeline automates the entire build and deployment process, simplifying the deployment process significantly.
#


## Expected Outcome
### APP IS WORKING , HERE IS THE FRONTEND WHICH CAN BE ACCESSED FROM THE PORT 3000
![todos_last](https://user-images.githubusercontent.com/114299019/221368082-9b109d09-3c4b-425e-9e73-834cb357827f.PNG)


#
This is a public project. Some secrets and personal information are hidden. When you install this project, you can change these fields with your personal information and secrets by using given list below.

1. Variable file-line 2-enter pem file name
2. Variable file- line 18- enter user name
3. Main.tf file- line 9- S3 bucket instance name
4. Main.tf file- line 24- enter user name
5. Main.tf file- line 31-enter pem file name
6. Docker_project.yaml file- line 6-95-118-137 - enter amazon account id
7. Jenkins_file- line 93- enter pem file name

