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
- In Jenkins for pipeline jobs, you can write script in **Groovy** language and no need to configure it like any other freestyle job.


## JenkinsFile
```Groovy
pipeline {
    agent any  // Runs the pipeline on any available agent

    environment {
        // Define environment variables
        PROJECT_NAME = 'my-project'
        BUILD_DIR = 'build'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from Git repository
                git 'https://github.com/user/my-project.git'
            }
        }

        stage('Build') {
            steps {
                // Run the build command, e.g., for a Maven project
                script {
                    // Make sure you're in the correct directory before building
                    dir(BUILD_DIR) {
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                // Run tests
                script {
                    // For example, using Maven to run tests
                    dir(BUILD_DIR) {
                        sh 'mvn test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the application if tests pass
                script {
                    echo 'Deploying to the staging server...'
                    // Add actual deployment command here (e.g., SCP, SSH, etc.)
                    sh './deploy.sh'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            // Clean up if needed
            echo 'Cleaning up...'
        }
    }
}
```
- pipeline: This is the root block of a declarative pipeline.
- agent any: Defines where the pipeline will run, in this case, any available agent.
- environment: You can define environment variables that will be available throughout the pipeline.
- stages: The pipeline is divided into multiple stages:
    - Checkout: Fetches the source code from a Git repository.
    - Build: Runs a build (Maven in this case).
    - Test: Runs tests using Maven.
    - Deploy: Deploys the application (a placeholder script here).
- post: This section defines actions that are executed after the pipeline runs:
    - success: Executed if the pipeline succeeds.
    - failure: Executed if the pipeline fails.
    - always: Executed regardless of success or failure (useful for cleanup tasks).