# Using AWS Services to Deploy Containers Using Continuous Delivery
Since AWS Cloud9 has been deprecated, an alternative to using the AWS CloudShell is to create a new EC2 instance and running the steps below there. 

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
11. 


Demo of FastAPI + AWS App Runner


## run docker
`docker build .`

Note this is your container name use:  `docker image ls` to find:

`docker run -p 127.0.0.1:8080:8080 54a55841624f`

![fastapi-step1](https://user-images.githubusercontent.com/58792/131587003-f5667c28-7cbe-402e-8795-f32a6ca9a4d1.png)
![fastapi-step2](https://user-images.githubusercontent.com/58792/131587286-341e795c-76dc-46a1-8ee9-528134410935.png)
![fastapi-step3](https://user-images.githubusercontent.com/58792/131587004-198ad6d5-2197-4de5-a6dd-4eb3c41e675e.png)
![fastapi-step4](https://user-images.githubusercontent.com/58792/131587005-866b0974-63d7-4fed-abf2-9c634721669f.png)


## Verify Swagger Working


![fastapi-swagger](https://user-images.githubusercontent.com/58792/131587676-b22c5877-0e75-49e7-a1a6-b580ba922e67.png)


## References

* [Watch on YouTube](https://youtu.be/XBBDqLf23Og)
* [Watch on O'Reilly](https://learning.oreilly.com/search/?query=author%3A%22Noah%20Gift%22&extended_publisher_data=true&highlight=true&include_assessments=false&include_case_studies=true&include_courses=true&include_playlists=true&include_collections=true&include_notebooks=true&include_sandboxes=true&include_scenarios=true&is_academic_institution_account=false&source=user&sort=relevance&facet_json=true&json_facets=true&page=0&include_facets=true&include_practice_exams=true)
