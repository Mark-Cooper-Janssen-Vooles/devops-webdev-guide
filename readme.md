# Devops Web Development 
This document is intended to cover the aspects required for devops processes. It generally follows the format of:
````
//contents section
  //gist cheatsheet  link
    //github repo example link
````
Let me know what you think, if I've missed anything or represented anything incorrectly - happy to hear any feedback!


Contents: 
#### Done
- AWS 3 day course: 20, 21, 22 October
- AWS exam certficiation https://aws.amazon.com/certification/certified-developer-associate/

#### Doing
- Host / hook up the front end and backend projects for quote-app on AWS! 
	- need backend, frontend, elastic beanstalk, dynamoDB, codebuild, github

---


https://confluence.teamxero.com/display/REP/2019/07/24/What+are+the+build+and+release+tools+for+XeroWeb+and+how+do+they+work

There's a lot of types of build pipelines using at Xero: Jenkins, Teamcity, CodePipeline but the one they are currently recommending is TeamCity. You can find the training for TC here: https://confluence.teamxero.com/display/PSP/Teamcity+Training

From a meeting I had with Paas team yesterday, they are thinking about building/moving to another pipeline tool in the near future and maybe make it as a standard pipelone across all dev teams. My recommendation is just try to understand briefly what pipelines is and how to use the type of pipeline your team is using. Any problems with TC you can ask #help-teamcity or just ping paas team directly :joy: In GSUP portfolio you can contact Delivery Engineer team for problems with your pipeline or modify anything in it.

Regarding using Kotlin for building TC: 
​go here for docs https://paas.xero.dev/docs#teamcity and https://github.dev.xero.com/Xero/teamcity-kotlin for code you can copy. The best way to learn is to use the teamcity-kotlin and build your own pipeline
Ping Lam if stuck


Useful guides to know what is the current best languages and frameworks:
- https://www.thoughtworks.com/radar/languages-and-frameworks  
- https://techradar.xero-support.com
​

---


## AWS
Amazon Web Services (AWS) is the world’s most comprehensive and broadly adopted cloud platform, offering over 175 fully featured services from data centers globally. Millions of customers—including the fastest-growing startups, largest enterprises, and leading government agencies—are using AWS to lower costs, become more agile, and innovate faster.

AWS Cheatsheet (long):
https://github.dev.xero.com/mark-janssen-vooles/awsdeveloperassociate

Miro board notes:
https://miro.com/app/board/o9J_lRJy4wY=/

Learn more about AWS here: 
https://aws.amazon.com/