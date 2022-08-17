# Devops Web Development 
This document is intended to cover the aspects required for devops processes. It generally follows the format of:
````
//contents section
  //gist cheatsheet  link
    //github repo example link
````

Contents: 
- [Docker](#docker)
- [Teamcity](#teamcity)
- [Infrastructure as code: Terraform](#terraform)
- [AWS](#aws)


---

Useful guides to know what is the current best languages and frameworks:
- https://www.thoughtworks.com/radar/languages-and-frameworks  
- https://techradar.xero-support.com
​

---
## Docker 
Docker is a way to containerise projects which reduces complexity and makes the development experience consistent accross the board regardless of the OS etc. 

80% of time in software projects is spent managing existing software, and only 20% on innovation. Docker reduces this maintenance time and allows us to innovate more. 

Notes on docker: 
https://github.com/Mark-Cooper-Janssen-Vooles/dockerMASTERY


---
## Teamcity 
Teamcity is a build management and continuous integration server from JetBrains.
Teamcity allows you to use Kotlin to codify the CI/CD pipeline and its configuration. 

More info here: https://www.jetbrains.com/teamcity/


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

More info here: https://www.terraform.io/

---
## AWS
Amazon Web Services (AWS) is the world’s most adopted cloud platform, offering many services from data centers globally. It has millions of customers - most tech companies would either use this or Azure. AWS can help to lower costs, become more agile, and innovate faster, and implement microservices.

AWS Cheatsheet (long):
https://github.dev.xero.com/mark-janssen-vooles/awsdeveloperassociate

Miro board notes:
https://miro.com/app/board/o9J_lRJy4wY=/

Learn more about AWS here: 
https://aws.amazon.com/