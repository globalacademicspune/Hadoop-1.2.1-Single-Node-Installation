# Hadoop 1.2.1 Single Node Installation Guide

This guide explains how to install and configure Apache Hadoop 1.2.1 on Ubuntu in pseudo-distributed mode.

> **Note:** Hadoop 1.2.1 is an obsolete release. Use it for learning and compatibility testing, not production deployments.

## Prerequisites

- Ubuntu system
- Sudo privileges
- Internet connectivity
- Compatible Java 8 installation

---

## Step 1: Update Package Repository

```bash
sudo apt-get update
```

---

## Step 2: Configure SSH

Install SSH packages:

```bash
sudo apt-get install openssh-client openssh-server -y
sudo systemctl enable --now ssh
```

Generate an SSH key:

```bash
ssh-keygen -t rsa
```

Configure passwordless SSH for localhost:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Test the connection:

```bash
ssh localhost
```

Exit the SSH session:

```bash
exit
```

---

## Step 3: Install Java 8

```bash
sudo apt-get install openjdk-8-jdk -y
```

Verify Java:

```bash
java -version
```

Check the Java installation path:

```bash
readlink -f "$(which java)"
```

A common Java 8 path on 64-bit Ubuntu is:

```text
/usr/lib/jvm/java-8-openjdk-amd64
```

If Java 8 is unavailable in your Ubuntu repositories, install a compatible Java 8 distribution and adjust the path accordingly.

---

## Step 4: Download Hadoop 1.2.1

Move to the home directory:

```bash
cd ~
```

Download Hadoop:

```bash
wget https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1.tar.gz
```

Extract the archive:

```bash
tar -xzvf hadoop-1.2.1.tar.gz
```

Move Hadoop to `/usr/local/hadoop`:

```bash
sudo mv hadoop-1.2.1 /usr/local/hadoop
```

Set ownership for the current user:

```bash
sudo chown -R "$USER":"$(id -gn)" /usr/local/hadoop
```

---

## Step 5: Configure Environment Variables

Open `.bashrc`:

```bash
vi ~/.bashrc
```

Add these lines at the end:

```bash
export HADOOP_PREFIX=/usr/local/hadoop
export HADOOP_HOME=/usr/local/hadoop
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH="$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin"
```

Save and exit:

```text
:wq
```

Reload the environment:

```bash
source ~/.bashrc
```

Verify:

```bash
echo "$JAVA_HOME"
echo "$HADOOP_HOME"
hadoop version
```

---

## Step 6: Configure Hadoop Environment

Open the configuration file:

```bash
vi /usr/local/hadoop/conf/hadoop-env.sh
```

Set the following variables:

```bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_OPTS="-Djava.net.preferIPv4Stack=true"
```

Save and exit.

---

## Step 7: Configure core-site.xml

Open the file:

```bash
vi /usr/local/hadoop/conf/core-site.xml
```

Configure the file as follows:

```xml
<?xml version="1.0"?>
<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>

    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/hadoop/tmp</value>
    </property>
</configuration>
```

Save and exit.

---

## Step 8: Configure hdfs-site.xml

Open the file:

```bash
vi /usr/local/hadoop/conf/hdfs-site.xml
```

Configure replication for a single-node setup:

```xml
<?xml version="1.0"?>
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

Save and exit.

---

## Step 9: Configure mapred-site.xml

Open the file:

```bash
vi /usr/local/hadoop/conf/mapred-site.xml
```

Configure the JobTracker:

```xml
<?xml version="1.0"?>
<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>localhost:9001</value>
    </property>
</configuration>
```

Save and exit.

---

## Step 10: Create Hadoop Temporary Directory

```bash
mkdir -p /usr/local/hadoop/tmp
chmod 700 /usr/local/hadoop/tmp
```

---

## Step 11: Reload Environment Variables

```bash
source ~/.bashrc
```

Verify Hadoop commands:

```bash
which hadoop
which start-all.sh
```

---

## Step 12: Format the NameNode

> **Warning:** Run this command only when initializing a new HDFS namespace. Formatting an existing NameNode can make existing HDFS metadata inaccessible.

```bash
hadoop namenode -format
```

Confirm that the command completes successfully.

---

## Step 13: Start Hadoop Services

```bash
start-all.sh
```

---

## Step 14: Verify Hadoop Daemons

```bash
jps
```

Expected Hadoop daemons:

```text
NameNode
DataNode
SecondaryNameNode
JobTracker
TaskTracker
```

`Jps` may also appear. Process IDs and output order will vary.

---

## Troubleshooting

### Check SSH Connectivity

```bash
ssh localhost
```

### Check Java Configuration

```bash
echo "$JAVA_HOME"
java -version
```

### Check Hadoop Logs

```bash
ls -l /usr/local/hadoop/logs
```

Review the relevant daemon log if a service fails to start.

### Stop Hadoop Services

```bash
stop-all.sh
```

---

## Final Result

Hadoop 1.2.1 is configured in pseudo-distributed mode on a single Ubuntu machine. After successful startup, the NameNode, DataNode, SecondaryNameNode, JobTracker, and TaskTracker should be running.

For production environments, use a supported Hadoop release and its corresponding documentation.
