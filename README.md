# 🚀 Apache Tomcat 10 Installation on Amazon Linux 2023 (AWS EC2)

This repository provides a step-by-step guide to install and configure **Apache Tomcat 10** on an **Amazon Linux 2023 EC2 instance**.

## 📋 Prerequisites

* AWS EC2 Instance (Amazon Linux 2023)
* SSH Access
* Security Group allowing:

  * SSH (22)
  * HTTP (8080)

---

# Step 1: Update the System

```bash
sudo dnf update -y
```

---

# Step 2: Install Java 21

```bash
sudo dnf install java-21-amazon-corretto -y
```

Verify the installation:

```bash
java -version
```

---

# Step 3: Navigate to `/opt`

```bash
cd /opt
```

---

# Step 4: Download Apache Tomcat

```bash
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.57/bin/apache-tomcat-10.1.57.tar.gz
```

---

# Step 5: Extract Tomcat

```bash
sudo tar -xvzf apache-tomcat-10.1.44.tar.gz
```

Rename the extracted directory:

```bash
sudo mv apache-tomcat-10.1.44 tomcat
```

---

# Step 6: Create a Tomcat User

```bash
sudo useradd -r -m -d /opt/tomcat -s /bin/false tomcat
```

---

# Step 7: Change Ownership

```bash
sudo chown -R tomcat:tomcat /opt/tomcat
```

---

# Step 8: Give Execute Permission

```bash
sudo chmod +x /opt/tomcat/bin/*.sh
```

---

# Step 9: Configure Tomcat Users

Edit the configuration file:

```bash
sudo vi /opt/tomcat/conf/tomcat-users.xml
```

Replace the contents with:

```xml
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="host-manager-gui"/>

<user username="admin" password="admin123" roles="manager-gui,admin-gui,host-manager-gui"/>

</tomcat-users>
```

> **Note:** For production environments, use a strong password instead of the example above.

---

# Step 10: Allow Remote Access to Manager App

Edit:

```bash
sudo vi /opt/tomcat/webapps/manager/META-INF/context.xml
```

Comment out the following section:

```xml
<!--
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1"/>
-->
```

---

# Step 11: Allow Remote Access to Host Manager

Edit:

```bash
sudo vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
```

Comment out the same `<Valve>` section.

---

# Step 12: Start Tomcat

```bash
sudo -u tomcat /opt/tomcat/bin/startup.sh
```

---

# Step 13: Verify Tomcat

Check the Tomcat process:

```bash
ps -ef | grep tomcat
```

Check if port 8080 is listening:

```bash
ss -tulnp | grep 8080
```

---

# Step 14: Configure EC2 Security Group

Allow inbound traffic on port **8080**.

| Type       | Protocol | Port | Source                 |
| ---------- | -------- | ---- | ---------------------- |
| Custom TCP | TCP      | 8080 | 0.0.0.0/0 (or your IP) |

---

# Step 15: Access Tomcat

Tomcat Home:

```
http://<EC2-Public-IP>:8080
```

Tomcat Manager:

```
http://<EC2-Public-IP>:8080/manager/html
```

Host Manager:

```
http://<EC2-Public-IP>:8080/host-manager/html
```

---

# Step 16: Login Credentials

```
Username: admin
Password: admin123
```

---

# Step 17: Stop Tomcat

```bash
sudo -u tomcat /opt/tomcat/bin/shutdown.sh
```

---

# Step 18: Restart Tomcat

```bash
sudo -u tomcat /opt/tomcat/bin/shutdown.sh
sudo -u tomcat /opt/tomcat/bin/startup.sh
```

---

# Step 19: Deploy a WAR File

Copy the application:

```bash
cp application.war /opt/tomcat/webapps/
```

Tomcat automatically deploys the application.

Access it using:

```
http://<EC2-Public-IP>:8080/application
```

---


---

# 🎯 Technologies Used

* Amazon Linux 2023
* Apache Tomcat 10
* Java 21 (Amazon Corretto)
* AWS EC2

---

# ⭐ If you found this repository helpful, consider giving it a Star!#hello
