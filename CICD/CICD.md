## Build Automation
- The process of performing tasks automatically of building (compiling, testing, packaging, and deploying) of software applications.
- Improves efficiency, collaboration, combined testing and QA between teams.

## Jenkins
- It is software for build automation and widely used by the industry
- You can run test, build artifacts, publish and deploy artifacts, send
notification to teams about the build process and also has an UI interface for visualizing build process.
- It has plugins for integration with Container (Docker), build tools, repositories, deployment servers, etc platforms. Example, code hosted on gitlab, build docker image of java app with gradle, push to nexus repo and deploy to ec2 instance.
- docker command to run jenkins on a server: `docker run -p 8080:8080 -p 50000:50000 -v jensins_home:/var/jenkins_home jenkins/jenkins:lts`. 8080 port is for accessing jenkin application and 50000 port is where jenkins master and worker nodes communicate, so jenkins can run as a cluster too.
- When you run and access the jenkins application, it will ask you to unlock the application using initialAdminPassword which you can find in container at path /var/jenkins_home/secrets/initialAdminPassword using cat.
- There are 2 roles in jenkins
    1. Jenkins Administrator: administers and manages Jenkins, sets up jenkins cluster, installs plugins and backups jenkins data. This can be Operations or Devops teams.
    2. Jenkins User: Creating actual jobs to run workflows. Development or Devops team.
- To need to install and configure different tools on jenkins to run test and build application package, for example, A Nodejs app would require npm to be available on jenkins or in case of Java app would require Maven to be available and installed on jenkins.
- You can install them from jenkin plugin UI or directly ssh to container where jenkins is running and install the tools.
