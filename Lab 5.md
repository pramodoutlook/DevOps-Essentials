## Configuring `Docker Machine` as Jenkins Slave, build and deploy code in Docker Host as a container

### Task-1: Configuring Docker Machine as Jenkins Slave.

1. In Jenkins Webpage, Navigate to **Jenkin's Dashboard** and click on the **Manage Jenkins** and **Nodes**.
2. Click on **New Node** in the next window. Give the node name as `docker-slave` and Select `Permanent Agent`
3. Fill out the details for the node docker-slave as given below.
   * The name should be given as **docker-slave**
   * Remote Root Directory as **/home/ubuntu**
   * labels to be **Slave-Nodes**
   * usage to be given as **"use this node as much as possible"**
   * Launch method to be set as **"launch agents via SSH"**.
   * In the host section, give the **Public IP of the Docker instance**.
   * For Credentials for this Docker node, click on the dropdown button named **Add** and then click on **Jenkins**
   * Then in the next window, in kind select **SSH username with private key** (Give username as `ubuntu`)
   * In **Private Key** Select **Enter directly**
   
   **Note:** To get the `Private Key` go to `Jenkins Server` and Execute the below command:
      ```
      cat /home/ubuntu/.ssh/id_rsa
      ```
     (Copy the entire content of the Private Key, including the **First and Last line** till `5 hyphens` only.)
     
   * Once Copied, Paste it into the space provided for the **private key** then click on **Add**..
   * Now, In SSH Credentials, choose the newly created **Ubuntu** credentials.
   * Host Key Verification Strategy Select as **Non Verifying Verification Strategy** and **Save** it and it's done.

**Note:** Check whether the Slave Node is online/Offline.

   <details>
     <summary>(Optional Step): If still the slave node is offline, do as below:</summary>
     
   1. From Jenkin's server using Docker's Public IP SSH into Docker server.
   2. Now, Re-check whether the Slave node is Online/Offline.
   </details>

### Task-3: Build and deploy code in Docker Host on the container.

Once the Slave Node is Online, then continue the below.

1. Navigate to your **Jenkins Home page**, choose the **hello-world project** from the drop-down, and click on the Configure tab.
2. In the **General Tab**, choose **Restrict where this project can be run** and set the Label Expression to **Slave-Nodes.**
3. Move to the **Post Build Steps Tab**, select **"Run only if the build succeeds,"** and then select `add a post-build step`, choose **Execute shell** from the drop-down, paste the provided commands (below) into the shell, and click **Save.**

#### Note: You may replace 'yourname' with your actual first name (lines 3 and 5).

```
cd ~
cp -f /home/ubuntu/workspace/hello-world/target/hello-world-war-1.0.0.war .
sudo docker container rm -f helloworld-container
sudo docker build -t helloworld-image .
sudo docker run -d -p 8080:8080 --name helloworld-container helloworld-image
```
   <details>
     <summary>Click here for breakup of command</summary>
     
   The commands you provided are part of the Jenkins job's post-build steps, and they are responsible for building a Docker image and running a Docker container for your Java web application. Here's a breakdown of what each command does:
   
   1. `cd ~`: Change the working directory to the user's home directory.
   
   2. `cp -f /home/ubuntu/workspace/hello-world/target/hello-world-war-1.0.0.war .`: Copy the WAR file (presumably the artifact of your Java web application) from the Jenkins workspace to the current directory (`~`), where you'll perform the Docker build.
   
   3. `sudo docker container rm -f yourname-helloworld-container`: Remove any existing Docker container with the name "yourname-helloworld-container" forcefully if it exists. You should replace "yourname" with your actual first name.
   
   4. `sudo docker build -t helloworld-image .`: Build a Docker image with the tag "helloworld-image" based on the Dockerfile located in the current directory (`.`). The Dockerfile you created earlier specifies how the image should be built.
   
   5. `sudo docker run -d -p 8080:8080 --name yourname-helloworld-container helloworld-image`: Run a Docker container named "yourname-helloworld-container" from the "helloworld-image" image. This container will be detached (`-d`) and will map port 8080 on the host to port 8080 inside the container. You should replace "yourname" with your actual first name.
      
   </details>

Upon executing these commands, your Java web application should be deployed within a Docker container, accessible on port 8080 of your Docker host.

### Task 4: Building the **hello-world project**

Either:
1. Manually click on **"Build Now"** in Jenkins.
2. Make a small change in GitHub files.

After a successful build, access the Tomcat server page:

In your browser, type **"http:// < Your Docker Host Public IP >:8080/hello-world-war-1.0.0/"** to view the website.
*Example:* http://3.95.192.77:8080/hello-world-war-1.0.0/

---------------------------------------------------------------------
Once Done, It's time to **Clean up** the Instances

We can now **terminate all the 3 instances** and we are Done with All Labs.


