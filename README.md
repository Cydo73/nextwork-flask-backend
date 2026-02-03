<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Set Up Kubernetes Deployment

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-eks2)

**Author:** Mumpe Cydrone  
**Email:** cydronem@gmail.com

---

## Set Up Kubernetes Deployment

![Image](http://learn.nextwork.org/warm_turquoise_kind_hoiho/uploads/aws-compute-eks2_45e6c3de5)

---

## Introducing Today's Project!

In this project, I will:
1. Clone a backend application from GitHub.
2. Build a Docker image of the backend.
3. Push your image to an Amazon ECR repository.
4. Troubleshoot installation and configuration errors - as a PRO student, I'm ready to face errors head-on and push my skills to the next level.
 because we are setting up a kubernetes development right here

### Tools and concepts

I used Amazon EKS, Git... to... Key steps include...
1. Launch an EC2
2.Connect to my EC2
3. Install kubernetes and launch cluster on my ec2
4.Pull code from a github repo
5.Build a container image of my app's backend
6.Push my container image to Amazon ECR for easy retrieval, access and deployment

### Project reflection

This project took me approximately...2h/3hrs. The most challenging part was troubleshooting the errors I got because my user wasn't in group Docker. My favourite part was finally pushing my code to Amazon ECR

Something new that I learnt from this experience was that docker is configured to be used only by the root user. You have to add your user to group Docker to get that priviledge.

---

## What I'm deploying

To set up today's project, I launched a Kubernetes cluster. Steps I took to do this included...
1. launching an ec2
2.connecting to my ec2
3.installing kubernetes using the terminal
4.setting up a cluster using the terminal

### I'm deploying an app's backend

Next, I retrieved the backend that I plan to deploy. An app's backend means the logical or systems keeping the app's components running like the databases, the UI components, the functions and systems. I retrieved backend code by running a command to clone the git repo in from the github remote location to my EC2

![Image](http://learn.nextwork.org/warm_turquoise_kind_hoiho/uploads/aws-compute-eks2_1ebb86c71)

---

## Building a container image

Once I cloned the backend code, my next step is to build a container image of the backend. This is because like i said, Kubernetes needs a docker image inorder to deploy the application's backend code onto different environments.

When I tried to build a Docker image of the backend, I ran into a permissions error because docker is supposed to be used by the root user, not the alias account (ec2-user) which i used to launch and connect the EC2.
The Docker commands I ran before this worked because they're prefixed with sudo, which lets a non-root user run commands with root user rights. But, it's good practice to give my ec2-user the permission instead of using sudo each time.

To solve the permissions error, I typed the command "sudo usermod -a -G docker ec2-user" into the terminal, which added my user (ec2-user) to the docker group. The Docker group is a group of users that are allowed to send commands to docker.
By default, only the root user can run Docker commands. When you add a user (e.g., ec2-user) to the Docker group, it lets that user run Docker commands without typing sudo every time.

![Image](http://learn.nextwork.org/warm_turquoise_kind_hoiho/uploads/aws-compute-eks2_45e6c3de5)

---

## Container Registry

I'm using Amazon ECR in this project to store my container images for easy retrieval and deployment. ECR is a good choice for the job because it automatically allows kubernetes to retrieve and deploy my container image.
Amazon ECR (Elastic Container Registry) is a container registry service by AWS, which means you use it to securely store, share, and deploy container images.

Container registries like Amazon ECR are great for Kubernetes deployment because Amazon ECR (Elastic Container Registry) is a container registry service by AWS, which means you use it to securely store, share, and deploy container images.

![Image](http://learn.nextwork.org/warm_turquoise_kind_hoiho/uploads/aws-compute-eks2_l2m3n4o5)

---

## EXTRA: Backend Explained

After reviewing the app's backend code, I've learnt that in summary,
1. My app's backend fetches data based on a search topic I enter.

2. The backend code uses Flask to connect with an external API (Hacker News Search API) and process the data.

3. The backend then sends the data back as formatted JSON.

### Unpacking three key backend files

The requirements.txt file lists all the requirements needed for my application. The packages, dependencies, etc

The Dockerfile gives Docker instructions on how to actually build my application's container image. 
Key commands in this Dockerfile include:
FROM python:3.9-alpine sets up the environment for your app by using Python 3.9 on Alpine Linux, which is a very lightweight version of Linux. This keeps your app size small, making it faster to build and run.
LABEL Author="NextWork" adds author metadata to the image.
WORKDIR /app sets /app as the working directory in the container, so the commands in the rest of this Dockerfile will be run from there.
COPY requirements.txt requirements.txt copies the requirements.txt file from your local machine (i.e. your EC2 instance) into the container's /app directory.
RUN pip3 install -r requirements.txt installs the dependencies in requirements.txt.
COPY . . copies all the files from your current directory into /app dir.
CMD ["python3", "app.py"] says the container should run the command python3 app.py to start your Flask app.

The app.py file contains the app's main code. It has 3 main parts:
1.Setting up the app and routing: The code starts by importing the Flask framework and tools for creating an API. It defines a route (/contents/<topic>) that directs incoming requests to the SearchContents class. This means users visiting your backend will need to visit a URL that ends with /contents/<topic> to see any results

2. Fetching data: In the SearchContents class, the get() method extracts the topic from the URL, sends a request to the Hacker News Search API to find related content, and collects important details like id, title, and url.

3. Sending the response: The collected data is turned into a formatted JSON response.


---

---

