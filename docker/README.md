# Assignment: Pulling and Running a Simple Container in AWS Cloud9

In this hands-on activity, we'll guide you through the process of accessing the AWS Cloud Shell, pulling a simple Docker container from Docker Hub, and running it on AWS Cloud Shell.


## Execute Basic Docker commands using Cloud9 
### 1
```bash
docker pull hello-world
```
This command fetches the `hello-world` container image from Docker Hub.

### 2
```bash
docker images
```
Check for the `hello-world` image in the output list.

### 3
```bash
docker run hello-world
```
Run the container.

## Create Docker image and save it in the ECR
### 1
In your Cloud9 or local environment, create a new file named `Dockerfile`:

```bash
touch Dockerfile
```
Open the Dockerfile in an editor and add the following content:
```bash
# Use an official Python runtime as the base image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory's contents into the container at /app
COPY . /app

# Define an environment variable
ENV NAME=World

# Run a simple Python script when the container launches
CMD ["python", "-c", "print(f'Hello, $NAME!')"]
```

This Dockerfile describes a simple container that prints "Hello, World!" using Python.

### 2
Build the Docker Image
```bash
docker build -t hello-world .
```

### 3
Create an ECR Repository 
```bash
aws ecr create-repository --repository-name hello-world
```
### 4
Authenticate Docker to the ECR registry:
```bash
aws ecr get-login-password --region your-region-name | docker login --username AWS --password-stdin your-account-id.dkr.ecr.your-region-name.amazonaws.com
```
Note - Replace `your-region-name` with your AWS region, for example `ap-southeast-1`, and `your-account-id` with your AWS account ID.

### 5
Tag your image for the ECR repository:
```bash
docker tag hello-world:latest your-account-id.dkr.ecr.your-region-name.amazonaws.com/hello-world:latest
```
Note - Replace your-account-id with your AWS account ID and your-region-name with your AWS region.
### 6
Now, push the image:
```bash
docker push your-account-id.dkr.ecr.your-region-name.amazonaws.com/hello-world:latest
```