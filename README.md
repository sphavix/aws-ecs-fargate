# Deploy a NodeJS App to Amazon ECS and Fargate

This blog demonstrates deploying a simple application to Amazon ECS and Fargate. Amazon Elastic Container Service (ECS) is a fully managed container orchestration service that helps you to deploy, manage, and scale containerized applications more efficiently. This blog assumes that you have all the prerequisites in place.
### Prerequisites 
  - AWS Account.
  - EC2 Instance with Docker and AWS CLI installed.

### Clone the Source Code from GitHub

    git clone https://github.com/pmgoriya/node-todo-cicd.git

  ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/e1dbdb01-c3bf-4b45-a862-20f8d2f38b6c)

### 2. Build the Docker Image
The source code includes a Dockerfile. Build the docker images from the docker file by navigating to the source code directory and running the command below:
  sudo docker build -t todoapp .
  
  ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/f7bde1d0-d14d-4df1-a423-164fe0dbc852)


### 3. Create an Amazon Elastic Container Registry
Now, login to the AWS Management Console, go to Elastic Container Registry and create a public repository that will hold the docker image created in Step 2. Click Create Repository.

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/c6ee5d2f-8135-4917-9b19-22db69e7ac17)

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/58bfd1de-dd75-4cdc-a7af-e94d03349a0d)



### 4. Push the Docker Image to ECR
After creating the repository, follow the steps below to authenticate and push the docker image to the ECR repository that you just created.
Get the commands from the ECR repository:
Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/b4b53c12-7e09-45ff-a165-8ace23a50cd9)

  sudo aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws/r5h2p7m7
  
![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/c45ae614-5d70-4de0-b04e-99eb13ee62ad)

 - Tag your image so you can push the image to this repository:
    sudo docker tag todoapp:latest public.ecr.aws/r5h2p7m7/todoapp:latest
    ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/580c7854-bede-4292-a94c-e5c6c1732e07)

  - Run the following command to push this image to your newly created AWS repository:
    sudo docker push public.ecr.aws/r5h2p7m7/todo-app:latest
    
    ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/a9ce69aa-7d53-4e1a-9b3c-bd58b18c26d9)

    
Our image has been pushed to Amazon Elastic Container Registry
![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/f3fffc4b-6248-46f4-b497-236cb51b2800)



### 5. Create a Cluster in the Amazon Elastic Container Service
Now we need to create the ECS Cluster where our service will reside. In the AWS Management Console, navigate to Elastic Container Service and create a Cluster select AWS Fargate (serverless) infrastructure. Provide the cluster name and select AWS Fargate, then click the Create button.

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/20b58474-b25a-46dc-9626-faeff4b503af)


The cluster has been successfully created.
![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/122287dd-ba88-462e-b903-e4a9538538b9)




### 6. Create a Task Definition
Under the Amazon ECS console, navigate to Task Definitions and click Create new task definition.
  - Enter the task definition name
    ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/cc898845-dbea-43e7-b9d3-f742e693db82)


  - Under Infrastructure requirements, select AWS Fargate launch type, Linux/X86_64 Operating System architecture. For task size, select 1 vCPU and 2GB Memory. Select None for the task role and Create new role under the Task execution role section.
    ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/fa187270-94c2-47b8-90af-f1048ce3031d)

  - Specify a name, container image, and whether the container should be marked as essential. Each task definition must have at least one essential container. Add port mappings to allow the container to access ports on the host to send or receive traffic. Leave everything as default and click Create.
    ![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/3b2d59be-bdd3-4fe5-9a93-2f3dad0ccee1)




### 7. Deploy the Task definition and Create service
Under the Environment section, select an existing cluster that we created earlier and leave everything as default.

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/bd393e24-e4b1-414b-99a6-bfba18094f0c)


Under the Deployment configuration section, Specify what type of application you want to run provide the service name, and click Create.

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/2e57eb35-0372-4325-9a4d-7f78fd274e38)

The Service has been created and is up and running.

![image](https://github.com/sphavix/aws-ecs-fargate/assets/103431152/5c075e7d-897c-4c3c-ab9d-0ef98bd5e0d9)
