# jenkins-homework

Jenkins homework

## Tasks

* connect static slave node
* create declarative job
* add parameter environment
* trigger on push and pr
* skip build if commit message is "SKIP_CI"
* create zip file with suffix $BRANCH_NAME and store it like artifact and build_number
* create shared library to send slack notification with build status
* in parallel ping 3 different servers and if ping failed - stop the job
* move all logic to shared library
* ADDITIONAL: after building app, Jenkins should tag the current commit with build number

## my actions

1. Create VMs with Jenkins in GCP
2. Installed necessary plugins, plus GCE plugin
3. Created a multibranch pipeline, added SCM with Jenkinsfile
    ![img1](./img/Screenshot%202021-09-30%20213549.png)
4. Configured GCE plugin to create worker instances based on an image with java installed (otherwise Jenkins didn't want to connect with the nodes)
    ![img2](./img/Screenshot%202021-09-30%20213739.png)
5. Added a when condition so "build" stage is skipped when commit message equals to "SKIP_CI"
   ![img3](./img/Screenshot%202021-09-30%20223739.png)
6. As a sample project to build, I use a HTML5 boilerplate, just npm project
7. Last stage - archiving artifacts built by npm in zip file
8. Added github webhook
9. Finally! jenkins builds website and adds zipped /dist/ folder to artifacts
    ![img4](/img/Screenshot%202021-10-06%20015846.png)
