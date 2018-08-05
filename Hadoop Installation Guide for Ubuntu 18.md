# Hadoop Installation Guide for Ubuntu 18

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
$ sudo apt-get install ssh
$ sudo apt-get install rsync
```

> Now we have ssh and rsync.  This will be used to allow your master node to interact with your slave nodes.  I will show how that works later on.

