# Using AWS Services to Deploy Containers Using Continuous Delivery
Since AWS Cloud9 has been deprecated, an alternative to using the AWS CloudShell is to create a new EC2 instance and running the steps below. 

## Deploying a Container In EC2 Instance
### Setup EC2 Instance
1. After launching EC2 instance, click on the EC2 ID on the console > Connect. Follow the instructions under the 'SSH Client' tab. Navigate to the VScode and download the Remote SSH client. Connect to the EC2 instance via CMD + Shift + P and Click the name of the EC2 instance.
   
2. Run the following commands after opening terminal of EC2 instance on VScode:
```sudo yum update -y
   sudo yum install -y git 
   sudo yum install -y make 
   sudo yum install -y virtualenv
   sudo yum install -y docker
```

3. Create a virtual environment by running ```virtualenv ~/.venv``` and go to ```~/.bashrc``` and add ```source ~/.venv/bin/activate``` to the end of the file, save it and run ```source ~/.bashrc``` on the terminal. (You should see your virtualenv on the left of your terminal CLI).

### Setup Github SSH Key 
4. Setup your SSH key for your Github. Run `ssh-keygen -t rsa` in the terminal and press Enter for all commands. Copy the path under the line `Your public key has been saved...` and run `cat [path_copied]` and copy the output. Go to your Settings in Github > SSH and GPG Keys > New SSH key > Paste output under 'Key' 

5. Fork this repository and clone it into your EC2 instance using the HTTPS method and run `make install`
 
### Setup AWS configure and Docker configuration file
6. Run the following command:
```sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -aG docker ec2-user
sudo yum install -y aws-cli
```
7. Run `docker info`. If you get an error such *ERROR: permission denied while trying to connect to the Docker daemon socket*, then run the following commands:

```
   sudo usermod -aG docker ec2-user
   newgrp docker
   groups
   ls -l /var/run/docker.sock
```
- The output should state 'srw-rw---- 1 root docker 0 Aug 30 08:00 /var/run/docker.sock'
- Run `docker info` again to make sure there is no error. 

8. Create an Access key in your AWS console. Copy the Access key and Secret key to a text file. Run `aws configure` and paste the keys into the terminal. Select default region (in my case, eur-west-1) and default output as json.
   
### AWS Elastic Container Registry and Building/Pushing Docker Image to ECR
9. Create a private repository under AWS ECR and go to `View push commands` and follow the instructions listed under the 'Mac/Linux' tab. Check that your docker container is on ECR by going to the AWS ECR console and checking for an image. 

### AWS App Runner
10. Go to AWS App runner console and create a new AWS App Runner service. Browse for your container image URI and select it. Choose an `automatic` deployment trigger and create a new service role (named ContinuousContainerFastAPI), click Next, give your service a name and create the service.

### AWS CodeBuild
11. Create a CodeBuild project. Select your source as the Github repository that you selected in the initial steps. (Make sure your credentials have been setup). Select `Rebuild every time a code change is pushed to this repository`. Under Buildspec, you can use a preset `buildspec.yaml` file. 

