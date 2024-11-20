# Devops Web Development 
This document is intended to cover the aspects required for devops processes. It generally follows the format of:
````
//contents section
  //gist cheatsheet  link
    //github repo example link
````

Contents: 
- [Teamcity](#teamcity)
- [Docker](#docker)
- [Infrastructure as code: Terraform](#infrastructure-as-code-terraform)
- [Kubernetes/K8s and Heml](#kubernetes-and-helm)
- [AWS](#aws)
  - [Exposing API on EC2](#exposing-an-api-on-an-ec2)

---

Useful guides to know what is the current best languages and frameworks:
- https://www.thoughtworks.com/radar/languages-and-frameworks  
- https://techradar.xero-support.com
​

---
## Teamcity 
Teamcity is a build management and continuous integration server from JetBrains.
Teamcity allows you to use Kotlin to codify the CI/CD pipeline and its configuration. 

More info here: 
- https://www.jetbrains.com/teamcity/


---
## Docker 
Docker is a way to containerise projects which reduces complexity and makes the development experience consistent accross the board regardless of the OS etc. 

80% of time in software projects is spent managing existing software, and only 20% on innovation. Docker reduces this maintenance time and allows us to innovate more. 

Notes on docker: 
- https://github.com/Mark-Cooper-Janssen-Vooles/dockerMASTERY


---
## Infrastructure as code: Terraform
Terraform is a tool for provisioning:
Day 1 - go from running nothing to running something 
Day 2+ - evolving existing infrastructure 
Day N - i.e. decomissioning the infrastructure

Takes an "infrastructure as code" approach

Allows you to declaratively define in a set of terraform config files, what you want your infrastructure to look like. 

Declarative = define WHAT end result you want.

You might incrementally evolve your infrastructure over time. 
Its captured in the terraform configuration by describing each piece of infrastructure. You can reference them within the tf config. 

Commands:
- Refresh => reconciles what tf things the world looks like, with the real world. 
- Plan => tf figures out what we're running and what we want to be running (our desired state based on the config) 
- Apply => executes plan against the real world, i.e works out the right order in which our infrastructure needs to be done. 
- Destroy => creates a special destroy plan, destroys all infrastructure in real world


Terraform's architecture:
- TF Config and TF State get fed into the "CORE" 
- TF has many different providers, i.e. AWS or Azure (IaaS - infrastructure as a service), lambda (PaaS), datadog (SaaS). It is best at IaaS.

Terraform is an open source project with thousands of contributiors so the list of providers is constantly growing.

Workflow: 
1. single dev writes a tf config 
2. they run the plan command, it says whats going to be done
3. run the apply command
4. cycle repeats 

if we have multiple people working on the tf config: we use 'terraform enterprise' or TFE, where we push the plan to something like github, and then TFE runs the plan so its managed centrally and only 1 run at a time so its sequential.

Terraform modules: 
Terraform files might have expert early operators (ie devops team who sets it up) and then later on is more exposed to standard devs. 

a tf module is a "black box" of infrastructure. we can define a set of input variables, and it emits a set of output variables => we can encapsulate the complexity to make it easier to deal with.

We can have a registery where theres a catalogue / 'module registry' of templates on how to do various things. I.e. AWS might have a recommended way, but also terraform enterprise provides this - a company might have their own approved modules set up by a devops team. 

What is the difference between Ansible and Terraform? 
They're both "infrastructure as code" 
Terraform is maining a provisioing tool, but it can deploy apps. Relatively new. More advanced in orchestration. 
Ansible is mainly a configuration tool (i.e. once infrasturcutre is already provisioned). Been around for longer.
(You can use both at once)

More info here: 
- https://www.terraform.io/

---

## Kubernetes and Helm 
- Kubernetes, usually referred to as k8s is an open-source platform designed for automating the deployment, scaling and management of containerized applications.
- Helm is a package manager for k8s that simplifies the deployment and management of apps on K8s using "charts". Charts are pre-configured reusable templates that define the resources needed for an app. 

- While K8s is ideal for managing and orchestrating containerized applications and microservices, Terraform (above) is used for setting up infrastructure such as servers, networking, databases, load balancers and more. 

More info here: 
- https://github.com/Mark-Cooper-Janssen-Vooles/k8s-training

---

## AWS
Amazon Web Services (AWS) is the world’s most adopted cloud platform, offering many services from data centers globally. It has millions of customers - most tech companies would either use this or Azure. AWS can help to lower costs, become more agile, and innovate faster, and implement microservices.

- Miro board notes (the best way to view!):
https://miro.com/app/board/uXjVMUp_CK8=/ 
- AWS Cheatsheet (long):
https://github.dev.xero.com/mark-janssen-vooles/awsdeveloperassociate
- Learn more about AWS here: 
https://aws.amazon.com/

----

### Exposing an API on an EC2 

When we try to expose our application to the web in production, we need to set up a web server.

This example shows how to do that using .net 6, Krestel, aws EC2, and linux. Inspired by this [blog](https://nodogmablog.bryanhogan.net/2021/12/how-to-run-net-6-kestrel-and-web-api-on-an-aws-ec2-linux-instance/).

1. Create an EC2 in AWS 
  - add security group which allows incoming http and https traffic to certain ports 
  - there should be an SSH one for 0.0.0.0/0 => used to ssh into the linux terminal
  - there should be a HTTPS (port 443) / HTTP (port 80) => for 0.0.0.0/0 and ::/0 (both http and https)
  - if any custom ones as well, i.e. port range 5000-5001 for http for 0.0.0.0/0 will expose that port (and will be used later)
2. Connect to the instance and install whatever is required for your app
  - In this case, install .net 6 
    - confirm its installed by running `dotnet --version`
3. Create a web API
  - run these commands:
  ````
dotnet new webapi --name HelloFromLinuxInstance
dotnet run --project HelloFromLinuxInstance --urls "http://*:5000;https://*:5001"
  ````
  - open a new connection to the instance and run `wget localhost:5000/weatherforecast --no-check-certificate`
  - use `cat weatherforecsast` to open the file create above and view if it worked. if it does, it means Kestrel (inbuilt web server with aws) is working 
4. Connect to the web API application from outside 
  - in 1. we added port 5000-5001 as exposed for http (0.0.0.0/0)
  - we should now be able to visit our API in the browser by going to: http://ec2-x-xx-xxx-xxx.compute-1.amazonaws.com:5000/weatherforecast 
5. If using something like AWS CodeDeploy, this will need to be run in daemon mode, i.e. by using something like `pm2`. An example of a project that does this is [Todolendar.API](https://github.com/Mark-Cooper-Janssen-Vooles/Todolendar.API)
  - The appspec.yml is used by CodeDeploy
  - It first runs `deploy-scripts/before_install.sh`, which installs pm2. Then `start_server.sh` and `stop_server.sh` start and stop the application using pm2. 