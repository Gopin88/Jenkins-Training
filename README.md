# Jenkins-Zero-To-Hero

Are you looking forward to learn Jenkins right from Zero(installation) to Hero(Build end to end pipelines)? then you are at the right place. 

## Installation on EC2 Instance

YouTube Video ->
https://www.youtube.com/watch?v=zZfhAXfBvVA&list=RDCMUCnnQ3ybuyFdzvgv2Ky5jnAA&index=1


![Screenshot 2023-02-01 at 5 46 14 PM](https://user-images.githubusercontent.com/43399466/216040281-6c8b89c3-8c22-4620-ad1c-8edd78eb31ae.png)

Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">


### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.


----------------------------------------------------->

The log message indicates that the version of Java currently installed on your system is **Java 11**, but Jenkins requires **Java 17 or Java 21** to run properly.

### To fix this issue:

1. **Install Java 17 or Java 21**:
   Since Jenkins supports Java 17 and Java 21, let's install one of these versions. Here’s how to install Java 17:

   ```bash
   sudo apt-get update
   sudo apt-get install openjdk-17-jdk
   ```

2. **Set Java 17 as the default**:
   After installation, you need to set Java 17 as the default Java version on your system:

   ```bash
   sudo update-alternatives --config java
   ```

   This will display a list of installed Java versions. Enter the number corresponding to Java 17 and press Enter.

3. **Verify the Java version**:
   After setting Java 17 as the default, verify that it's active:

   ```bash
   java -version
   ```

   It should now show Java 17 as the default version.

4. **Restart Jenkins**:
   Now that you have the correct Java version installed and set, try restarting the Jenkins service:

   ```bash
   sudo systemctl restart jenkins
   ```

5. **Check Jenkins status**:
   After restarting Jenkins, check if the service is running successfully:

   ```bash
   sudo systemctl status jenkins
   ```

This should resolve the issue with Jenkins not starting due to the Java version mismatch.


To remove Java 11 from your system after installing Java 17, you can follow these steps:

### 1. **Uninstall Java 11**:
   You can remove Java 11 using the following command:

   ```bash
   sudo apt-get remove --purge openjdk-11-jdk
   ```

   This will uninstall the Java 11 package.

### 2. **Verify Java Versions**:
   After uninstalling Java 11, check which versions of Java are still available on your system:

   ```bash
   update-alternatives --config java
   ```

   Make sure Java 17 is still selected as the default. If it’s not, you can select it using the command above.

### 3. **Clean Up Unused Packages**:
   It’s a good idea to clean up any residual files from the uninstallation to free up space:

   ```bash
   sudo apt-get autoremove
   sudo apt-get clean
   ```

This should remove Java 11 and leave you with only the version you want (Java 17, in this case).

### 4. **Verify Java Version**:
   Finally, check that your system is now using the correct Java version:

   ```bash
   java -version
   ```

It should now show Java 17 (or whichever version you installed) as the active version.








