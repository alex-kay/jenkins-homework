# jenkins-homework

Jenkins homework

## Tasks

* connect static slave node **(TO DO)**
* create declarative job *(done)*
* add parameter environment *(done)*
* trigger on push and pr *(done with wbhook)*
* skip build if commit message is "SKIP_CI" *(done)*
* create zip file with suffix $BRANCH_NAME *(done)*
* and store it like artifact and build_number *(done)*
* create shared library to send slack notification with build status **(TO DO)**
* in parallel ping 3 different servers and if ping failed - stop the job *(done)*
* move all logic to shared library *(almost)*
* ADDITIONAL: after building app, Jenkins should tag the current commit with build number *(done)*

## my actions

1. Create VMs with Jenkins in GCP
2. Installed necessary plugins, plus GCE plugin
3. Created a multibranch pipeline, added SCM with Jenkinsfile
    ![img1](./img/Screenshot%202021-09-30%20213549.png)
4. Configured GCE plugin to create worker instances based on an image with java installed (otherwise Jenkins didn't want to connect with the nodes)
    ![img2](./img/Screenshot%202021-09-30%20213739.png)
5. Added a when condition so "build" stage is skipped when commit message equals to "SKIP_CI"
   ![img3](./img/Screenshot%202021-09-30%20223739.png)
6. As a sample project to build, I used a [HTML5 boilerplate](https://github.com/h5bp/html5-boilerplate), just npm project (i utilized my previous code - [a project from external EPAM training course](https://github.com/alex-kay/html5-boilerplate/blob/master/Jenkinsfile))
7. Last stage - archiving artifacts built by npm in zip file
8. Added github webhook
9. Finally! jenkins builds website and adds zipped /dist/ folder to artifacts
    ![img4](/img/Screenshot%202021-10-06%20015846.png)
10. Moved all logic to [library](https://github.com/alex-kay/jenkins-shared-lib), only some steps left in Jenkinsfile.
11. Added shared library to Jenkins in configuration.
    ![img5](/img/Screenshot%202021-10-06%20111503.png)
