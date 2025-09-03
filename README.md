# jenkinsclassnotes
***
## Jenkins Home Directory Notes

- The **Jenkins Home Directory** is the main folder where Jenkins stores **all its data**:  
  - Job configurations  
  - Build logs  
  - Plugin data  
  - Workspace directories  
  - Build artifacts  
  - System settings and more

- On Linux machines, the default Jenkins Home directory is:  
  **`/var/lib/jenkins`**

- **Backing up Jenkins means backing up the Jenkins Home directory.**

- To **change the Jenkins Home directory location**, do the following:
  1. Stop the Jenkins service.  
  2. Create the new directory you want to use for Jenkins Home.  
  3. Copy all contents from the old Jenkins Home directory to the new one.  
  4. Change ownership of the new directory to the Jenkins user.  
  5. Set the environment variable `JENKINS_HOME` to point to the new directory in Jenkins configuration (e.g., edit `/etc/default/jenkins`).  
  6. Restart Jenkins service.

- Changing Jenkins Home may be needed if:  
  - You want to store Jenkins data on a different disk or partition with more space.  
  - Your current Jenkins Home directory is running out of space.  
  - Organizational or project requirements dictate a custom directory.


### NOTE: 
- To change the ownership of a directory to the Jenkins user on a Linux system, use the `chown` command as follows:

 ```bash
 sudo chown -R jenkins:jenkins /path/to/newjenkinshome
 ```

Explanation:
- `sudo`: Run the command with superuser privileges.
- `chown`: Change ownership command.
- `-R`: Recursive, applies ownership change to all files and subdirectories inside the directory.
- `jenkins:jenkins`: This sets the user to `jenkins` and the group to `jenkins`.
- `/path/to/newjenkinshome`: Replace this with the actual path to the new Jenkins home directory.

This ensures Jenkins user has full ownership and access to the directory and all its contents.

***

## Jenkins Project and Workspace Notes

1. **Project Folder Creation:**  
   When a new Jenkins project (job) is created, a folder is created inside:  
   `/var/lib/jenkins/jobs/<project-name>`  
   This folder stores the project/job’s configuration files and build history.

2. **Workspace Folder:**  
   Each project/job has a **workspace directory** where the actual job execution happens:  
   `/var/lib/jenkins/workspace/<project-name>`  
   This is where Jenkins checks out the source code from version control and stores files generated during the build.

3. **Jenkins Permissions:**  
   - Jenkins runs as a system user, typically named `jenkins`.  
   - Jenkins can perform any actions that the system user `jenkins` has permissions for on the server.  
   - To allow Jenkins to perform actions requiring elevated privileges, add the `jenkins` user to the sudoers group by editing the sudoers file:  
     ```
     jenkins ALL=(ALL:ALL) NOPASSWD: ALL
     ```
   - This allows Jenkins to run commands using `sudo` without a password prompt, enabling installations, deployments, and other admin tasks.

4. **Summary:**  
   - `/var/lib/jenkins/jobs`: stores job config & build history  
   - `/var/lib/jenkins/workspace`: holds files during job execution  
   - Jenkins user permissions control what Jenkins can do on the system  
   - Grant sudo privileges carefully to allow secure elevated access

***

Here are clear notes on the sections of a Jenkins Freestyle Project:

***

## Jenkins Freestyle Project Sections

1. **General:**  
   Basic project information such as the project name and description.

2. **Source Code Management (SCM):**  
   Configure where to get the source code (e.g., Git, SVN). You provide repository URLs and credentials here.

3. **Build Triggers:**  
   Conditions for automatic build starts, such as polling SCM changes, scheduled builds (cron), or triggered by other projects.

4. **Build Environment:**  
   Settings that affect the build environment like preparing the workspace, injecting environment variables, or running wrappers.

5. **Build Steps:**  
   These define the individual tasks Jenkins performs during the build, for example:  
   - Execute shell commands  
   - Run batch scripts  
   - Invoke Maven or Gradle builds  
   - Run tests

6. **Post Build Actions:**  
   Tasks to perform after the build completes, such as:  
   - Archiving artifacts  
   - Sending notifications (email, Slack)  
   - Deploying build outputs  
   - Triggering other jobs

### NOTE: 
"Invoke Maven or Gradle builds" means Jenkins uses these tools to run the build process of your project. Here’s a clear explanation:

***

## What is Maven and Gradle?

- **Maven** and **Gradle** are popular **build automation tools** mainly used in Java projects.
- They help automate compiling source code, running tests, packaging binaries (like JAR files), and managing project dependencies (libraries).
- Instead of running individual commands manually, these tools use configuration files (`pom.xml` for Maven, `build.gradle` for Gradle) to define how the project should be built.

## What does "Invoke Maven or Gradle builds" mean in Jenkins?

- Jenkins can execute Maven or Gradle commands as part of a build step in your project.
- For example: Jenkins may run `mvn clean install` or `gradle build` to compile code, run tests, and create build artifacts.
- Jenkins integrates with Maven and Gradle through plugins that allow easier configuration and displaying build results.

## Why use this feature?

- **Automation:** Automatically build your project on code changes.
- **Continuous Integration:** Run builds, tests, and package software automatically.
- **Standardized builds:** Use Maven/Gradle best practices within Jenkins.
- **Dependency management:** Automatically download required libraries during build.
- **Integration:** Jenkins plugins enhance build scan reporting and monitoring.
***
In short, "Invoke Maven or Gradle builds" is Jenkins running the standard automated build commands of your Java (or similar) project, ensuring your software is built and tested continuously and reliably.
***


