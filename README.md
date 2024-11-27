# Hadoop Lab Setup and Configuration

This repository contains my experience setting up and configuring a functional single-node Hadoop environment as part of a Big Data lab exercise. The setup was performed in an Ubuntu environment (via WSL2 under Windows), and the repository includes configuration files, setup steps, troubleshooting solutions, and insights.

---

## 🚀 Project Overview

### Objective
- Configure Hadoop 3.4.0 on a single-node environment.
- Ensure the Hadoop Distributed File System (HDFS) is functional.
- Demonstrate understanding of core Hadoop concepts such as NameNode, DataNode, and resource management.

---

## 🔧 Key Features
- Successfully formatted the NameNode and configured core Hadoop files.
- Set up the web interface to monitor HDFS at `http://localhost:9870`.
- Generated SSH keys to enable passwordless SSH, a prerequisite for multi-node setups.

---

## 🛠️ Prerequisites

Before starting, ensure you have the following installed:

- **Ubuntu** (via WSL, VirtualBox, or native installation)
- **Java 8 (OpenJDK)**: Required for Hadoop
- **Hadoop 3.4.0 binaries**
- **SSH** (installed by default in most Linux distributions)

---

## 📂 Repository Structure

hadoop-lab-setup/ ├── config/ # Contains modified configuration files ├── scripts/ # (Optional) Automation scripts for setup ├── screenshots/ # Images showcasing the working environment ├── data/ # Placeholder for input/output data ├── README.md # Project overview and instructions └── notes.md # Additional insights and troubleshooting notes

yaml

---

## 📜 Setup Instructions

### 1. Download and Extract Hadoop
Download the Hadoop 3.4.0 binary compatible with your environment (e.g., `amd64`) and extract it:
```bash
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz
tar -xzf hadoop-3.4.0.tar.gz
mv hadoop-3.4.0 ~/hadoop
2. Update ~/.bashrc
Edit your ~/.bashrc file to add Hadoop and Java paths:
```

```bash
export HADOOP_HOME=~/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Reload the file:

```bash
source ~/.bashrc
```

3. Configure Hadoop Files
Modify the following files located in ~/hadoop/etc/hadoop/:
```bash
core-site.xml
```
xml

```bash
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```

hadoop-env.sh
Ensure the JAVA_HOME variable is set:
```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
mapred-site.xml
```

Rename the template file and modify it:

```bash
mv mapred-site.xml.template mapred-site.xml
```

xml

```
<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```
```bash
yarn-site.xml
```

xml
```
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
</configuration>
```

4. Format the NameNode
Run the following command to format the HDFS:

```bash
hdfs namenode -format
```

5. Set Up SSH
Generate SSH keys for passwordless SSH:

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

6. Start Hadoop Services
Start the HDFS and YARN services:

```bash
start-dfs.sh
start-yarn.sh
```

7. Access the Web Interface
Open your browser and navigate to:

HDFS Overview: http://localhost:9870
YARN Resource Manager: http://localhost:8088
📸 Screenshots
Screenshots showcasing the HDFS health overview and web interface functionality are located in the screenshots/ directory.

---

## 🌐 In a Nutshell
1. Hadoop Binary Download: I initially downloaded the aarch64 version of Hadoop, but I discovered it only works under a real or virtual machine running Ubuntu. Therefore, I switched to downloading the regular binary version of Hadoop 3.4 since I was using Ubuntu in terminal (under windows).

2. Configuration Modifications: I made several modifications to ensure proper functionality:

* ~/.bashrc: Updated to include necessary environment variables.
* hadoop-env.sh: Configured to set java home ( JAVA_HOME  )  and other required settings.
* core-site.xml: Set fs.defaultFS to hdfs://localhost:9000.
* mapred-site.xml: Configured with mapreduce.framework.name set to yarn.
* HDFS Formatting and Starting Services: After configuring the files, I ran the command bin/hdfs namenode -format to create the necessary directory structure and initialize the HDFS.

3. Public Key and Password Setup: I ensured passwordless SSH access was set up for seamless communication between the master and slave nodes, facilitating easier management.

4. Starting HDFS: I executed the command start-dfs.sh to initiate the Hadoop Distributed File System.

5. Web Interface Verification: Finally, I accessed the Hadoop web interface by opening http://localhost:9870 in my browser, where I confirmed that the NameNode was active and functioning correctly.


---


📖 Key Insights
Why SSH?: SSH enables seamless communication between Hadoop nodes. Even in single-node setups, this is a requirement for starting Hadoop services.
File Configurations: Configuration files are crucial to customizing the Hadoop environment, including defining paths and frameworks (e.g., YARN for resource management).
Troubleshooting: Encountered issues with incompatible binaries (e.g., aarch64) and resolved them by downloading the correct binary.


🤔 How to Extend This Project?
Multi-Node Cluster: Add slave machines to create a distributed setup.
Run Jobs: Execute sample MapReduce jobs on this setup.
Integrate with Hive: Use Hive to query HDFS data.
