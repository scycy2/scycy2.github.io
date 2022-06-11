---
title: Use Docker to configure three nodes cluster
date: 2022-06-10 12:57:09
tags: ['Big Data', 'Hadoop', 'Centos', 'Docker']
categories: ['self-study notes']
cover:
summary: The steps to configure hadoop cluster using docker
img:
---

#### 1. Pull centos image and install prerequisites

* pull the latest centos version

  ```bash
  docker pull centos:latest
  ```

* Create a new container and run it

  * Instead of using the most common command, we should use the command below:

    ```bash
    docker run -itd -p 9870:9870 -p 8088:8088 --privileged=true centos:latest /sbin/init
    ```

    Using this command allows us to use 'systemctl' command in centos, we will use this to start ssh service later. We also map the ports we will use later.

  * Then we interacts with the container using the terminal

    ```bash
    docker ps -a # to see all containers' info, and find id
    
    docker exec -it containerID /bin/bash
    ```

    We will enter the container successfully.

* Install Java

  * Create one folder for Java

    ```bash
    mkdir /usr/java
    ```

  * Download JDK1.8 from website https://www.oracle.com/java/technologies/downloads/

  * Copy the tar or tar.gz file into the container (before running below command, stop the container)

    ```bash
    docker cp jdk-8u333-linux-x64.tar containerID:/usr/java/
    ```

  * Then enter the container again, and unzip the tar or tar.gz file

    ```bash
    tar -xvf jdk-8u333-linux-x64.tar
    # tar -xgvf jdk-8u333-linux-x64.tar.gz
    rm -rf jdk-8u333-linux-x64.tar
    ```

  * Configure environment variables

    ```bash
    vi /etc/profile
    ```

    Add below lines to /etc/profile

    ```
    export JAVA_HOME=/usr/java/jdk1.8.0_333
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=${JAVA_HOME}/bin:$PATH
    ```

  * Enable environment variables

    ```bash
    source /etc/profile
    ```

  * Check java version

    ```bash
    java -version
    ```

    <img src="Use-Docker-to-configure-three-nodes-cluster/Screen Shot 2022-06-10 at 13.35.30.png" alt="Screen Shot 2022-06-10 at 13.35.30" style="zoom:50%;" />

* Install ssh service

  * First, when we use yum, some errors will occur, "Failed to download metadata for repo ‘AppStream’: Cannot prepare internal mirrorlist: No URLs in mirrorlist". Please follow below commands to update the source of yum to Ali image

    ```bash
    cd /etc/yum.repos.d/
    
    sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
    
    sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
    
    yum -y install wget
    
    wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
    
    yum clean all
    
    yum makecache
    ```

  * Then we can install ssh service

    ```bash
    yum install -y openssl openssh-server
    ```

  * Change ssh configuration, enable below lines

    ```
    Port 22
    PermitRootLogin yes
    PasswordAuthentication yes
    ```

  * Start ssh service

    ```bash
    yum install apt -y
    apt install systemctl
    systemctl start sshd.service
    systemctl enable sshd.service
    ```

  * Check if ssh is useful

    ```bash
    yum -y install openssh-clients
    yum -y install passwd
    passwd
    ssh localhost
    ```

* Install hadoop

  * The same as Java, download tar or tar.gz and copy to the container

    ```bash
    docker cp hadoop-3.3.3.tar ece5b3069513:/usr/hadoop
    ```

  * Unzip

    ```bash
    tar -xvf hadoop-3.3.3.tar
    ```

  * Configure environment variables

    ```bash
    # vi /etc/profile
    export HADOOP_HOME=/usr/hadoop/hadoop-3.3.3
    export  PATH=${HADOOP_HOME}/bin:$PATH
    ```

* Change Hadoop Configuration

  * First, configure /etc/hosts, add below lines

    <img src="Use-Docker-to-configure-three-nodes-cluster/Screen Shot 2022-06-10 at 13.56.24.png" alt="Screen Shot 2022-06-10 at 13.56.24" style="zoom:50%;" />

    

  * hadoop-env.sh

    ```bash
    export  JAVA_HOME=/usr/java/jdk1.8.0_333/
    ```

  * core-site.xml

    ```xml
    <configuration>
            <property>
                    <name>fs.defaultFS</name>
                    <value>hdfs://Master:8020</value>
            </property>
            <property>
                    <name>hadoop.tmp.dir</name>
                    <value>/usr/hadoop/hadoop-3.3.3/tmp</value>
            </property>
    </configuration>
    ```

  * hdfs-site.xml

    ```xml
    <configuration>
            <property>
                    <name>dfs.replication</name>
                    <value>3</value>
            </property>
            <property>
                    <name>dfs.namenode.datanode.registration.ip-hostname-check</name>
                    <value>false</value>
            </property>
            <property>
                    <name>dfs.namenode.name.dir</name>
                    <value>/usr/hadoop/hadoop-3.3.3/tmp/dfs/name</value>
            </property>
            <property>
                    <name>dfs.datanode.data.dir</name>
                    <value>/usr/hadoop/hadoop-3.3.3/tmp/dfs/data</value>
            </property>
    </configuration>
    ```

  * workers

    ```
    Master
    Worker1
    Worker2
    ```

  * mapred-site.xml

    ```xml
    <configuration>
            <property>
                    <name>mapreduce.framework.name</name>
                    <value>yarn</value>
            </property>
    </configuration>
    ```

  * yarn-site.xml

    ```xml
    <configuration>
    
            <property>
                    <name>yarn.nodemanager.aux-services</name>
                    <value>mapreduce_shuffle</value>
            </property>
            <property>
                    <name>yarn.resourcemanager.hostname</name>
                    <value>Master</value>
            </property>
    
    </configuration>
    ```

#### 2. Pack as image

* ```bash
  docker commit -a "authername" containerID imageName:tag
  ```

#### 3. Run two more containers

* Create and run two more containers based on the packed image
* Don't forget configure ip and hostname in /etc/hosts for all three containers

#### 4. Start HDFS, yarn and check status

* Initialization

  ```bash
  cd ${HADOOP_HOME}/bin/
  ./hdfs namenode -format
  ```

* Start HDFS and yarn

  ```bash
  cd ${HADOOP_HOME}/sbin/
  ./start-all.sh
  ```

* Run 'jps' in Master machine

  <img src="Use-Docker-to-configure-three-nodes-cluster/Screen Shot 2022-06-10 at 14.09.03.png" alt="Screen Shot 2022-06-10 at 14.09.03" style="zoom:50%;" />

* Run 'jps' in Worker machine

  <img src="Use-Docker-to-configure-three-nodes-cluster/Screen Shot 2022-06-10 at 14.10.04.png" alt="Screen Shot 2022-06-10 at 14.10.04" style="zoom:50%;" />

* Open 'localhost:9870'

  <img src="Use-Docker-to-configure-three-nodes-cluster/Screen Shot 2022-06-10 at 14.11.11.png" alt="Screen Shot 2022-06-10 at 14.11.11" style="zoom:50%;" />

  We can three live nodes

* Open 'localhost:8088'

  <img src="Use-Docker-to-configure-three-nodes-cluster/Screen Shot 2022-06-10 at 14.11.53.png" alt="Screen Shot 2022-06-10 at 14.11.53" style="zoom:50%;" />

  We can see three active nodes

#### 5. Using the configured image directly

* I have already pushed my image to docker hub, you can use my image directly

  ```bash
  docker pull 879314144/centoshadoop:Master
  ```

* Run three containers

  ```bash
  docker run -itd -p 9870:9870 -p 8088:8088 --privileged=true 879314144/centoshadoop:Master /sbin/init # Master, which maps ports to my local machine
  
  docker run -itd --privileged=true 879314144/centoshadoop:Master /sbin/init # Worker1
  
  docker run -itd --privileged=true 879314144/centoshadoop:Master /sbin/init # Worker2
  ```

* Enter containers and run hadoop

  ```bash
  docker exec -it containerID
  source /etc/profile
  cd ${HADOOP_HOME}/sbin/
  ./start-all.sh
  ```

* **Important!!! Every time you restarted the containers, some settings will fail, so I write some scripts to finish these settings automatically**

  * myStart.sh (in containers)

    ```bash
    #!/bin/sh
    echo "172.17.0.2      Master" >> /etc/hosts
    echo "172.17.0.3      Worker1" >> /etc/hosts
    echo "172.17.0.4      Worker2" >> /etc/hosts
    ```

    ```bash
    chmod +x myStart.sh # make the script executable, run this when the script is written for the first time
    ```

  * Add one line to ~/.bashrc

    ```bash
    source /etc/profile # make the environment varibles take effect each time the terminal starts
    ```

  * start_Master (Unix Executable File, as my laptop is MacBook) (outside the container)

    ```bash
    #!/bin/zsh
    docker start 1e0dc9a29fe7 # start container
    docker exec -it 1e0dc9a29fe7 /myStart.sh # run myStart.sh script
    docker exec -it 1e0dc9a29fe7 /bin/bash # interact with bash
    ```

    ```bash
    chmod +x start_Master # make the file executable
    ```

    Two same files for workers but with different container ID. If you do not need to interact with bash for two workers, you can put all in one file.

#### 6. Submit jar file to hadoop

* When I submit jar file to hadoop, the status is always **ACCEPTED**, which means the cluster cannot run the job. Then in logs I find that **UnknownHostException** as below:<img src="/Users/apple/Desktop/Screen Shot 2022-06-11 at 13.10.41.png" alt="Screen Shot 2022-06-11 at 13.10.41" style="zoom:50%;" />

  As nodes cannot recognize each other, so the communication between nodes fails.

* Therefore, we need add more lines to the script **myStart.sh**, that we mentioned above:

  ```bash
  #!/bin/sh
  echo "172.17.0.3      7be466059d81" >> /etc/hosts # new line
  echo "172.17.0.4      17c34ea417e8" >> /etc/hosts # new line
  echo "172.17.0.2      Master" >> /etc/hosts
  echo "172.17.0.3      Worker1" >> /etc/hosts
  echo "172.17.0.4      Worker2" >> /etc/hosts
  ```

  For all containers, we need to change the script, two added lines are hostname information about **another two containers**.

* Then we can submit jar files to hadoop:

  ```bash
  hadoop jar xxx.jar ClassName arg1 arg2 ....
  ```

  
