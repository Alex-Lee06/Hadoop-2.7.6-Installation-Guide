# Hadoop Installation Guide for Ubuntu 18 in Standalone Mode

This installation guide with walk you through on how to install Hadoop in a psuedo-distributed mode.

Below is a script that you will be able to run as well to install Hadoop-2.7.6 but the configurations for Hadoop will still need to be configured.

## Prerequisite before installing Hadoop Psuedo-Distributed is as follows.

* Java version 1.7.0 or 1.8.0

  > If you don't have a java version to install it is below

  ```
  $ sudo apt-get install default-jdk
  ```

  >  After you have installed jdk you can check your version again.

  ```
  $ java -version
  openjdk version "1.8.0_171"
  OpenJDK Runtime Environment (build 1.8.0_171-8u171-b11-0ubuntu0.18.04.1-b11)
  OpenJDK 64-Bit Server VM (build 25.171-b11, mixed mode)
  ```

* Next you can see if you have javac.  This should be the same version as your java

  ```
  $ javac -version
  ```

  > If you do not see a version you will get commands that shows you how to install it.
  >
  > Not sure how to install it? Follow the command below.

  ```
  $ sudo apt-get install default-jre
  ```

  > Once that is installed, to see the version use the following command.

  ```
  $ javac -version
  javac 1.8.0_171
  ```

> Now that you have both java and javac installed let's continue onto how to install Hadoop.

* First we need to install our ssh to the system.

```
$ sudo apt-get install openssh-server openssh-client
[sudo] password for hadoop: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
openssh-client is already the newest version (1:7.6p1-4).
openssh-server is already the newest version (1:7.6p1-4).
openssh-server set to manually installed.
The following packages were automatically installed and are no longer required:
  libgsoap-2.8.60 libvncserver1
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

```

> Now we have ssh.  This will be used to allow your master node to interact with your slave nodes.  I will show how that works later on.

* Let's generate a public key and a private key pair

```
$ ssh-keygen -t rsa -P ""
Generating public/private rsa key pair.
Enter file in which to save the key (/home/hadoop/.ssh/id_rsa): 
Your identification has been saved in /home/hadoop/.ssh/id_rsa.
Your public key has been saved in /home/hadoop/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Bs4sA8d47RqIr51KRTJwP43j9XjCGpLx/ToXOWSMoqU hadoop@hadoop
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|   o .           |
|+ + +oo          |
|.*o*.P+.         |
|++o.Xo*.S        |
|RO = O+.         |
|+ B *  o         |
|.L +...          |
|o + .+.          |
+----[SHG230]-----+

```

>Now we have generated a ssh key we need to configure ssh for a password-less key

```
$ cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```

>We have finally allowed access to your master node.  Now lets see if we can connect to the root user of the master node.

```
$ ssh localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA230:zkf2qIqwsmLfe9MrXTPLIk8Y9zK+ZQ5Kdi2+x3nJtZI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-29-generic x86_64)

```

> Now that we have our ssh working and can access our localhost let's exit out of it so that we can continue our Hadoop configurations.

```
$ exit
```

* With ssh installed and configured we can now download and install Hadoop-2.7.6. To do this let's go to http://hadoop.apache.org/releases.html and look for 2.7.6 binary
* Or you can follow the steps below for CLI commands.

```
wget http://apache.osuosl.org/hadoop/common/hadoop-2.7.6/hadoop-2.7.6.tar.gz
```

* Once the installation is completed lets extract and move your newly **hadoop-2.7.6** extraction to **/usr/local**

```
$ sudo mv hadoop-2.7.6 /usr/local
```

> We now need to change our directory to the path that **hadoop-2.7.6** is located.

```
$ cd /usr/local/hadoop-2.7.6
```

> Now that we have Hadoop in the location we want it to be. The next steps needs to be carefully executed.  We will begin configuring our ~/.bashrc file

```
$sudo gedit ~/.bashrc
```

> Gedit is just a simple text editor.  You can use whatever you like.

* Editing our **~/.bashrc file**.  I recommend typing this part in to learn, but you can copy and paste from here.

> ```
> export HADOOP_PREFIX=/home/hdadmin/hadoop-2.7.1
> export PATH=$PATH:$HADOOP_PREFIX/bin
> export PATH=$PATH:$HADOOP_PREFIX/sbin
> export HADOOP_MAPRED_HOME=${HADOOP_PREFIX}
> export HADOOP_COMMON_HOME=${HADOOP_PREFIX}
> export HADOOP_HDFS_HOME=${HADOOP_PREFIX}
> export YARN_HOME=${HADOOP_PREFIX}
> export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_PREFIX/lib/native
> export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX"
> ```

* Lets now edit the **hadoop-env.sh** to have it be able to locate your JAVA_HOME:

> export JAVA_HOME:{root-of-your-Java-installation}
>
> eg:
>
> export JAVA_HOME:/usr/lib/jvm/java-8.0.1-default

* Now that the environment for hadoop has been configured let's contine with the configurations for **core-site.xml**, **hdfs-site.xml**, **yarn-site.xml**, and **mapred-site.xml**

> I recommend installing Sublime Text so that all 4 **xml** files can be opened at the same time.  This will save you time.

* Let's first configure the **core-site.xml**. This is located in etc/hadoop inside the installation directory for Hadoop.  The needed configuration is below.

> 1. ```
>    <configuration>
>    <property>
>    <name>fs.defaultFS</name>
>    <value>hdfs://localhost:9000</value>
>    </property>
>    <property>
>    <name>hadoop.tmp.dir</name>
>    <value>/home/hdadmin/hdata</value>
>    </property>
>    </configuration>
>    ```

> After editing this file save it and move on to the **hdfs-site.xml**

* Let's configure the **hdfs-site.xml**. The location to this file is in etc/hadoop inside the installation directory for Hadoop.  The needed configuration is below.

> 1. ```
>    <configuration>
>    <property>
>    <name>dfs.replication</name>
>    <value>1</value>
>    </property>
>    </configuration>
>    ```

* Next is the configuration for **mapred-site.xml**. This file you will need to make a copy of **mapred-site.xml.template** naming it as above using **cp** after that you will need to give it rights to write to it using the  **chmod** command.  I will be giving it full rights **777** to keep things simple.

```
$ sudo cp mapred-site.xml.template mapred-site.xml
```

* After you have copied it open it using any text editor you want and add the configurations below to the file.

> 1. ```
>    <configuration>
>    <property>
>    <name>mapreduce.framework.name</name>
>    <value>yarn</value>
>    </property>
>    </configuration>
>    ```

We are almost done.  Next we need to configure the **yarn-site.xml**.  The location will be in /etc/hadoop inside of the installation of Hadoop like all the other files.  The configurations is as follows:

> 1. ```
>    <configuration>
>    <property>
>    <name>yarn.nodemanager.aux-services</name>
>    <value>mapreduce_shuffle</value>
>    </property>
>    <property>
>    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
>    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
>    </property>
>    </configuration>
>    ```



Once you have completed all those configurations you can finally start working with HDFS.  Before that though we need to format the namenode.  To do so the command is shown below.

> NOTE: make sure to be in /usr/local/hadoop-2.7.6/bin

```
/bin$ hdfs namenode -format
```

> NOTE: A little tip is to minimize having to format your namenode as it will delete your datanode.

You will see the following (This is what you should see near the bottom with an exit status of 0 if you see anything else there was an error.)

> 18/08/06 09:32:55 INFO namenode.FSImageFormatProtobuf: Image file /usr/local/hadoop-2.7.6/hdata/dfs/name/current/fsimage.ckpt_0000000000000000000 of size 321 bytes saved in 0 seconds.
> 18/08/06 09:32:55 INFO namenode.NNStorageRetentionManager: Going to retain 1 images with txid >= 0
> 18/08/06 09:32:55 INFO util.ExitUtil: Exiting with status 0
> 18/08/06 09:32:55 INFO namenode.NameNode: SHUTDOWN_MSG: 
> /************************************************************
> SHUTDOWN_MSG: Shutting down NameNode at hadoop/127.0.1.1
> ************************************************************/

Once that is complete successfully you can now run the following code.  You should be able to run this code anywhere from the terminal if not you will need to navigate to **/usr/local/hadoop-2.7.6/sbin**

```
$start-dfs.sh
```

The above command will start hdfs which has your **NameNode, SecondaryNameNode, DataNode**

Now you can start yarn.

```
$start-yarn.sh
```

This will start your **ResouceManage and NodeManager**

To see everything that is running use JPS

```
master@hadoop:/usr/local/hadoop-2.7.6/sbin$ jps
2865 NameNode
3603 NodeManager
3445 ResourceManager
3270 SecondaryNameNode
3655 Jps
3019 DataNode

```

## The installation of Hadoop 2.7.6 is complete you can now run map reduce jobs!!

