### 1. Installation of Java (Version 1.6 +)

> Note : Skip this if java is already installed

```bash
{wsangam@eldy ~ }
::sudo apt update
```

```bash
{wsangam@eldy ~ }
::sudo apt install openjdk-11-jdk
```

After installation, cross check it by below commands 

```bash
{wsangam@eldy ~ }
::java -version
openjdk version "11.0.10" 2021-01-19
OpenJDK Runtime Environment (build 11.0.10+9-Ubuntu-0ubuntu1.20.04)
OpenJDK 64-Bit Server VM (build 11.0.10+9-Ubuntu-0ubuntu1.20.04, mixed mode, sharing)

```

```bash
{wsangam@eldy ~ }
::which java
/usr/bin/java
```

```bash
{wsangam@eldy ~ }
::whereis java
java: /usr/bin/java /usr/share/java /usr/share/man/man1/java.1.gz
```

</br>

### 2. set up ssh certificate
Create ssh hostkey

```bash
{wsangam@eldy ~ }
::ssh-keygen -t rsa
```
Output :
![Screenshot from 2021-03-19 16-57-18.png](:/f97e29de59ed42b5b6d1aa85304ed1bd)

After that add generated public in authorised_keys

```bash
{wsangam@eldy ~ }
::cat .ssh/my_hadoop_single_node.pub >> .ssh/authorized_keys 
```

check wether generated key added in authorized_key
```
{wsangam@eldy ~ }
::cat .ssh/authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCjaRcXEUp8uXXdrgMa3+sLTgkLwgGLnB0jqV8AH+f/1XFnk3E62ny/ev3WkYa4TBxi6amGsB6aMZ+OMf+KneKTGdFt8/L7p7Etq1YgejzheqxGbibW+5tzvMyWahqADbIwWzZlMTlPMQrx8X/XvVEXSCTwBTP9VuuZX9m9zHUieJmHAhWwi/aaSmz2kqjsEtTm6WwgEz4f6l3avxsEfvcT2SWkL4bByRovT+xP+BnHXmXGu3WFq4KtSlivMC/6Z8dJFeW8E8UxONdUenLwOzMycURSIrjT8i13CNiq4kRjJAMcgBGCQfPOrT3k4nxL+tr5ZwVthkK1jHeju+Z7qf2hX+INOsByrezC0vQB3KeU9ohSWxzdSIlFIvZ9LV/c1qgnGW8EC2H2lV4EqG9YdHr4mwHsBzBLKUK3Y5bDp7OeuZQlU0xIPrcCq7DXwlRS/dIkf95Q7g2dQwtOOxeyG82/zf/kGsWBbnioQ+5coX+2TCm1o5wBOSHAEKtmkvMmzDM= wsangam@eldy
```

After this may need to restart terminal or machine


</br>

### 3. Download Hadoop-3.2.2 and extract it

You may download it from http://hadoop.apache.org/releases.html
Extract it and put it in /usr/local directory. (You can decide destination directory of your choice).
In my case I extracted it at /usr/local directory

move extracted file in /usr/local directory
```bash
{wsangam@eldy ~ }
::sudo mv hadoop-3.2.2/ /usr/local/
```
check wether it is moved and available in /usr/local directory

```bash
{wsangam@eldy ~ }
::ls /usr/local/
bin  etc  games  hadoop-3.2.2  include  lib  man  sbin  share  src
```

</br>

### 4. Set up envorionment variable

Now we need to setup envorionment variable for hadoop

Open .bashrc file
```bash
{wsangam@eldy ~ }
::vim .bashrc
```
At the end of the file add following line and change envorionment variable as per hadoop version and java jdk version. save it.
```bash
# Modify JAVA_HOME and HADOOP_INSTALL as per your requirements
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
export HADOOP_INSTALL=/usr/local/hadoop-3.2.2
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
# create alias
# dont need to run every time from '/usr/local/hadoop-3.2.2/sbin/start-all.sh'
alias hadoopAllStart='/usr/local/hadoop-3.2.2/sbin/start-all.sh'
alias hadoopAllStop='/usr/local/hadoop-3.2.2/sbin/stop-all.sh'
```
Apply made changes by
```bash
{wsangam@eldy ~ }
::source .bashrc
```
</br>

### 5. Open and Add java lib in path in /usr/local/hadoop-3.2.2/etc/hadoop/hadoop-env.sh
Open file
```bash
{wsangam@eldy ~ }
::sudo vim /usr/local/hadoop-3.2.2/etc/hadoop/hadoop-env.sh 
```
Find Line
```bash
export JAVA_HOME=${JAVA_HOME}
```
and replace it with
```bash
export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64

```
Java lib can be found here.
```bash
{wsangam@eldy ~ }
::ls /usr/lib/jvm/
default-java  java-1.11.0-openjdk-amd64  java-11-openjdk-amd64  openjdk-11
```





![Screenshot from 2021-03-19 17-08-08.png](:/6e8ebcdfb88742a9bcb8324b5e86c8ff)


</br>

### 6. Check hadoop version
You shall get similar to this

```bash
{wsangam@eldy ~ }
::hadoop version
```

![Screenshot from 2021-03-19 17-09-19.png](:/64c31360601b458e9e2509b61ff30231)

</br>

### 7. Configure xml files hadoop-3.2.2
Four files at directory /usr/local/hadoop-3.2.2/etc/hadoop need to be modified.
A. core-site.xml
B. mapred-site.xml
C. yarn-site.xml
D. hdfs-site.xml

#### A. core-site.xml

```bash
{wsangam@eldy ~ }
::sudo vim /usr/local/hadoop-3.2.2/etc/hadoop/core-site.xml
```
![Screenshot from 2021-03-19 17-12-19.png](:/ebef899a1da84b6bb4c534757809d1ef)

#### B. mapred-site.xml
```bash
{wsangam@eldy ~ }
::sudo vim /usr/local/hadoop-3.2.2/etc/hadoop/mapred-site.xml 
```
![Screenshot from 2021-03-19 17-14-18.png](:/dc733e634a9a4f37b52fc47ae66734f1)

#### C. yarn-site.xml
```bash
{wsangam@eldy ~ }
::sudo vim /usr/local/hadoop-3.2.2/etc/hadoop/yarn-site.xml
```
![Screenshot from 2021-03-19 17-15-55.png](:/bf13eeb2fb5040c0ab4bd69bed0283d3)

#### D. hdfs-site.xml
```bash
{wsangam@eldy ~ }
::sudo vim /usr/local/hadoop-3.2.2/etc/hadoop/hdfs-site.xml 
```
![Screenshot from 2021-03-19 17-18-09.png](:/18555b52f72b4ce485aab3528d83c1da)


</br>

### 8. Format namenode
As this is fresh installation, formatting name node will not harm your data, because it do not
contains any data. Formatting namenode will erase hadoop data. So just be sure when you format
hdfs next time.

```{wsangam@eldy ~ }
::hdfs namenode -format
2021-03-19 17:21:04,886 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = eldy/192.168.0.56
STARTUP_MSG:   args = [-format]
STARTUP_MSG:   version = 3.2.2
STARTUP_MSG:   classpath = /usr/local/hadoop-3.2.2/etc/hadoop:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jul-to-slf4j-1.7.25.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/audience-annotations-0.5.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/nimbus-jose-jwt-7.9.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jersey-json-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerby-pkix-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerby-asn1-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/protobuf-java-2.5.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-util-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jcip-annotations-1.0-1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/zookeeper-3.4.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-math3-3.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-core-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-lang3-3.7.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/curator-client-2.13.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/htrace-core4-4.1.0-incubating.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-identity-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/guava-27.0-jre.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/error_prone_annotations-2.2.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-admin-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/netty-3.10.6.Final.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/httpcore-4.4.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-compress-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/hadoop-annotations-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/javax.servlet-api-3.1.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-configuration2-2.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/httpclient-4.5.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-annotations-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jsr305-3.0.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerby-util-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-mapper-asl-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/woodstox-core-5.0.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/javax.activation-api-1.2.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/checker-qual-2.5.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/curator-framework-2.13.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/slf4j-api-1.7.25.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jaxb-impl-2.2.3-1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jettison-1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/asm-5.0.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-util-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-xml-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/failureaccess-1.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/gson-2.2.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-text-1.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-collections-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-io-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/j2objc-annotations-1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-server-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/hadoop-auth-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-codec-1.11.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jsch-0.1.55.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/animal-sniffer-annotations-1.17.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerby-xdr-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jsr311-api-1.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/token-provider-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-xc-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/dnsjava-2.1.7.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-cli-1.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jaxb-api-2.2.11.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/re2j-1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-server-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-client-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-simplekdc-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-http-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-logging-1.1.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-servlet-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/stax2-api-3.1.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jsp-api-2.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-databind-2.9.10.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/paranamer-2.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/avro-1.7.7.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-crypto-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-core-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-core-asl-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/accessors-smart-1.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jersey-server-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-io-2.5.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/curator-recipes-2.13.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-beanutils-1.9.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jersey-servlet-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/commons-net-3.6.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerb-common-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/kerby-config-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/json-smart-2.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jackson-jaxrs-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jersey-core-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-webapp-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/log4j-1.2.17.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/metrics-core-3.2.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/jetty-security-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/lib/snappy-java-1.0.5.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/hadoop-common-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/hadoop-common-3.2.2-tests.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/hadoop-nfs-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/common/hadoop-kms-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/audience-annotations-0.5.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/nimbus-jose-jwt-7.9.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jersey-json-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerby-pkix-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerby-asn1-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/protobuf-java-2.5.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-util-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jcip-annotations-1.0-1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/zookeeper-3.4.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-math3-3.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-core-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-lang3-3.7.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/curator-client-2.13.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/htrace-core4-4.1.0-incubating.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-util-ajax-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-identity-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/guava-27.0-jre.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/leveldbjni-all-1.8.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/error_prone_annotations-2.2.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-admin-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/netty-3.10.6.Final.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/httpcore-4.4.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-compress-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/hadoop-annotations-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/javax.servlet-api-3.1.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-configuration2-2.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/httpclient-4.5.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-annotations-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jsr305-3.0.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerby-util-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-mapper-asl-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/woodstox-core-5.0.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/javax.activation-api-1.2.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/checker-qual-2.5.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/curator-framework-2.13.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jaxb-impl-2.2.3-1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jettison-1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/asm-5.0.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-util-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-xml-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/failureaccess-1.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/gson-2.2.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-text-1.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-collections-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-io-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/j2objc-annotations-1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-server-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/hadoop-auth-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-codec-1.11.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jsch-0.1.55.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/animal-sniffer-annotations-1.17.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerby-xdr-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jsr311-api-1.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/token-provider-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-xc-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/dnsjava-2.1.7.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-cli-1.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jaxb-api-2.2.11.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/re2j-1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-server-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-client-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-simplekdc-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-http-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-logging-1.1.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-servlet-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/stax2-api-3.1.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-databind-2.9.10.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/paranamer-2.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/avro-1.7.7.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-crypto-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-core-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-core-asl-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/okio-1.6.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/accessors-smart-1.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jersey-server-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-io-2.5.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/curator-recipes-2.13.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-beanutils-1.9.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jersey-servlet-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-net-3.6.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerb-common-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/kerby-config-1.0.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/commons-daemon-1.0.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/json-smart-2.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jackson-jaxrs-1.9.13.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jersey-core-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/json-simple-1.1.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/okhttp-2.7.5.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-webapp-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/log4j-1.2.17.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/netty-all-4.1.48.Final.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/jetty-security-9.4.20.v20190813.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/lib/snappy-java-1.0.5.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-httpfs-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-nfs-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-rbf-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-native-client-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-client-3.2.2-tests.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-rbf-3.2.2-tests.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-client-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-native-client-3.2.2-tests.jar:/usr/local/hadoop-3.2.2/share/hadoop/hdfs/hadoop-hdfs-3.2.2-tests.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/lib/junit-4.11.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/lib/hamcrest-core-1.3.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-hs-plugins-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-shuffle-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-3.2.2-tests.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-app-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-uploader-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-hs-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-core-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-jobclient-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-common-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/mapreduce/hadoop-mapreduce-client-nativetask-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/java-util-1.9.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/bcprov-jdk15on-1.60.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/javax.inject-1.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/jersey-guice-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/guice-servlet-4.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/aopalliance-1.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/objenesis-1.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/jersey-client-1.19.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/guice-4.0.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/jackson-module-jaxb-annotations-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/snakeyaml-1.16.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/fst-2.50.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/HikariCP-java7-2.4.12.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/geronimo-jcache_1.0_spec-1.0-alpha-1.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/jackson-jaxrs-json-provider-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/ehcache-3.3.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/jackson-jaxrs-base-2.9.10.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/swagger-annotations-1.5.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/json-io-2.5.1.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/metrics-core-3.2.4.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/lib/bcpkix-jdk15on-1.60.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-web-proxy-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-registry-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-timeline-pluginstorage-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-api-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-applications-unmanaged-am-launcher-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-nodemanager-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-tests-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-common-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-router-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-client-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-sharedcachemanager-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-applicationhistoryservice-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-common-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-server-resourcemanager-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-submarine-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-applications-distributedshell-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-services-api-3.2.2.jar:/usr/local/hadoop-3.2.2/share/hadoop/yarn/hadoop-yarn-services-core-3.2.2.jar
STARTUP_MSG:   build = Unknown -r 7a3bc90b05f257c8ace2f76d74264906f0f7a932; compiled by 'hexiaoqiao' on 2021-01-03T09:26Z
STARTUP_MSG:   java = 11.0.10
************************************************************/
2021-03-19 17:21:04,900 INFO namenode.NameNode: registered UNIX signal handlers for [TERM, HUP, INT]
2021-03-19 17:21:05,008 INFO namenode.NameNode: createNameNode [-format]
Formatting using clusterid: CID-4344f450-5bf7-4cce-8697-a06476f1321c
2021-03-19 17:21:05,616 INFO namenode.FSEditLog: Edit logging is async:true
2021-03-19 17:21:05,651 INFO namenode.FSNamesystem: KeyProvider: null
2021-03-19 17:21:05,653 INFO namenode.FSNamesystem: fsLock is fair: true
2021-03-19 17:21:05,653 INFO namenode.FSNamesystem: Detailed lock hold time metrics enabled: false
2021-03-19 17:21:05,703 INFO namenode.FSNamesystem: fsOwner             = wsangam (auth:SIMPLE)
2021-03-19 17:21:05,703 INFO namenode.FSNamesystem: supergroup          = supergroup
2021-03-19 17:21:05,703 INFO namenode.FSNamesystem: isPermissionEnabled = true
2021-03-19 17:21:05,703 INFO namenode.FSNamesystem: HA Enabled: false
2021-03-19 17:21:05,754 INFO common.Util: dfs.datanode.fileio.profiling.sampling.percentage set to 0. Disabling file IO profiling
2021-03-19 17:21:05,767 INFO blockmanagement.DatanodeManager: dfs.block.invalidate.limit: configured=1000, counted=60, effected=1000
2021-03-19 17:21:05,767 INFO blockmanagement.DatanodeManager: dfs.namenode.datanode.registration.ip-hostname-check=true
2021-03-19 17:21:05,772 INFO blockmanagement.BlockManager: dfs.namenode.startup.delay.block.deletion.sec is set to 000:00:00:00.000
2021-03-19 17:21:05,773 INFO blockmanagement.BlockManager: The block deletion will start around 2021 Mar 19 17:21:05
2021-03-19 17:21:05,775 INFO util.GSet: Computing capacity for map BlocksMap
2021-03-19 17:21:05,775 INFO util.GSet: VM type       = 64-bit
2021-03-19 17:21:05,778 INFO util.GSet: 2.0% max memory 1.7 GB = 34.8 MB
2021-03-19 17:21:05,778 INFO util.GSet: capacity      = 2^22 = 4194304 entries
2021-03-19 17:21:05,793 INFO blockmanagement.BlockManager: Storage policy satisfier is disabled
2021-03-19 17:21:05,793 INFO blockmanagement.BlockManager: dfs.block.access.token.enable = false
2021-03-19 17:21:05,798 INFO Configuration.deprecation: No unit for dfs.namenode.safemode.extension(30000) assuming MILLISECONDS
2021-03-19 17:21:05,798 INFO blockmanagement.BlockManagerSafeMode: dfs.namenode.safemode.threshold-pct = 0.9990000128746033
2021-03-19 17:21:05,798 INFO blockmanagement.BlockManagerSafeMode: dfs.namenode.safemode.min.datanodes = 0
2021-03-19 17:21:05,798 INFO blockmanagement.BlockManagerSafeMode: dfs.namenode.safemode.extension = 30000
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: defaultReplication         = 1
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: maxReplication             = 512
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: minReplication             = 1
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: maxReplicationStreams      = 2
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: redundancyRecheckInterval  = 3000ms
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: encryptDataTransfer        = false
2021-03-19 17:21:05,799 INFO blockmanagement.BlockManager: maxNumBlocksToLog          = 1000
2021-03-19 17:21:05,819 INFO namenode.FSDirectory: GLOBAL serial map: bits=29 maxEntries=536870911
2021-03-19 17:21:05,819 INFO namenode.FSDirectory: USER serial map: bits=24 maxEntries=16777215
2021-03-19 17:21:05,819 INFO namenode.FSDirectory: GROUP serial map: bits=24 maxEntries=16777215
2021-03-19 17:21:05,819 INFO namenode.FSDirectory: XATTR serial map: bits=24 maxEntries=16777215
2021-03-19 17:21:05,829 INFO util.GSet: Computing capacity for map INodeMap
2021-03-19 17:21:05,830 INFO util.GSet: VM type       = 64-bit
2021-03-19 17:21:05,830 INFO util.GSet: 1.0% max memory 1.7 GB = 17.4 MB
2021-03-19 17:21:05,830 INFO util.GSet: capacity      = 2^21 = 2097152 entries
2021-03-19 17:21:05,833 INFO namenode.FSDirectory: ACLs enabled? false
2021-03-19 17:21:05,833 INFO namenode.FSDirectory: POSIX ACL inheritance enabled? true
2021-03-19 17:21:05,833 INFO namenode.FSDirectory: XAttrs enabled? true
2021-03-19 17:21:05,833 INFO namenode.NameNode: Caching file names occurring more than 10 times
2021-03-19 17:21:05,838 INFO snapshot.SnapshotManager: Loaded config captureOpenFiles: false, skipCaptureAccessTimeOnlyChange: false, snapshotDiffAllowSnapRootDescendant: true, maxSnapshotLimit: 65536
2021-03-19 17:21:05,840 INFO snapshot.SnapshotManager: SkipList is disabled
2021-03-19 17:21:05,845 INFO util.GSet: Computing capacity for map cachedBlocks
2021-03-19 17:21:05,845 INFO util.GSet: VM type       = 64-bit
2021-03-19 17:21:05,845 INFO util.GSet: 0.25% max memory 1.7 GB = 4.4 MB
2021-03-19 17:21:05,845 INFO util.GSet: capacity      = 2^19 = 524288 entries
2021-03-19 17:21:05,853 INFO metrics.TopMetrics: NNTop conf: dfs.namenode.top.window.num.buckets = 10
2021-03-19 17:21:05,853 INFO metrics.TopMetrics: NNTop conf: dfs.namenode.top.num.users = 10
2021-03-19 17:21:05,853 INFO metrics.TopMetrics: NNTop conf: dfs.namenode.top.windows.minutes = 1,5,25
2021-03-19 17:21:05,856 INFO namenode.FSNamesystem: Retry cache on namenode is enabled
2021-03-19 17:21:05,857 INFO namenode.FSNamesystem: Retry cache will use 0.03 of total heap and retry cache entry expiry time is 600000 millis
2021-03-19 17:21:05,859 INFO util.GSet: Computing capacity for map NameNodeRetryCache
2021-03-19 17:21:05,859 INFO util.GSet: VM type       = 64-bit
2021-03-19 17:21:05,859 INFO util.GSet: 0.029999999329447746% max memory 1.7 GB = 535.1 KB
2021-03-19 17:21:05,859 INFO util.GSet: capacity      = 2^16 = 65536 entries
Re-format filesystem in Storage Directory root= /usr/local/hadoop-3.2.2/tmp/dfs/name; location= null ? (Y or N) Y
2021-03-19 17:21:19,543 INFO namenode.FSImage: Allocated new BlockPoolId: BP-1120173693-192.168.0.56-1616154679515
2021-03-19 17:21:19,544 INFO common.Storage: Will remove files: [/usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000009-0000000000000000009, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000004-0000000000000000004, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000002-0000000000000000003, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000016-0000000000000000017, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000013-0000000000000000014, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000021-0000000000000000033, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/fsimage_0000000000000000033, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000010-0000000000000000010, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000007-0000000000000000007, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000001-0000000000000000001, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/fsimage_0000000000000000033.md5, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000005-0000000000000000006, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000015-0000000000000000015, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000018-0000000000000000018, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/fsimage_0000000000000000072.md5, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/seen_txid, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/VERSION, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_inprogress_0000000000000000073, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000019-0000000000000000020, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000011-0000000000000000012, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000008-0000000000000000008, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/fsimage_0000000000000000072, /usr/local/hadoop-3.2.2/tmp/dfs/name/current/edits_0000000000000000034-0000000000000000072]
2021-03-19 17:21:19,574 INFO common.Storage: Storage directory /usr/local/hadoop-3.2.2/tmp/dfs/name has been successfully formatted.
2021-03-19 17:21:19,613 INFO namenode.FSImageFormatProtobuf: Saving image file /usr/local/hadoop-3.2.2/tmp/dfs/name/current/fsimage.ckpt_0000000000000000000 using no compression
2021-03-19 17:21:19,715 INFO namenode.FSImageFormatProtobuf: Image file /usr/local/hadoop-3.2.2/tmp/dfs/name/current/fsimage.ckpt_0000000000000000000 of size 402 bytes saved in 0 seconds .
2021-03-19 17:21:19,727 INFO namenode.NNStorageRetentionManager: Going to retain 1 images with txid >= 0
2021-03-19 17:21:19,735 INFO namenode.FSImage: FSImageSaver clean checkpoint: txid=0 when meet shutdown.
2021-03-19 17:21:19,736 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at eldy/192.168.0.56
************************************************************/
```
</br>

### 9. Start hadoop service.
Either you can start individual service or you can start all. These daemons are available at
/usr/local/hadoop-3.2.2/sbin

```
{wsangam@eldy /usr/local/hadoop-3.2.2/bin }
::cd /usr/local/hadoop-3.2.2/sbin/

{wsangam@eldy /usr/local/hadoop-3.2.2/sbin }
::./start-all.sh 
WARNING: Attempting to start all Apache Hadoop daemons as wsangam in 10 seconds.
WARNING: This is not a recommended production deployment configuration.
WARNING: Use CTRL-C to abort.
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [eldy]
Starting resourcemanager
Starting nodemanagers
```

But as we have created alias we can start by as follow
```
{wsangam@eldy ~ }
::hadoopAllStart 
WARNING: Attempting to start all Apache Hadoop daemons as wsangam in 10 seconds.
WARNING: This is not a recommended production deployment configuration.
WARNING: Use CTRL-C to abort.
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [eldy]
Starting resourcemanager
resourcemanager is running as process 29465.  Stop it first.
Starting nodemanagers
```


</br>

### 10. Hadoop configuration is done. To check the java process status run following

```
{wsangam@eldy ~ }
::jps
35793 SecondaryNameNode
35397 NameNode
29465 ResourceManager
36396 Jps
36111 NodeManager

```


</br>

### 11. To see Web UI of hadoop overview, put following in browser

http://localhost:9870

9870 	dfs.namenode.http-address 	0.0.0.0:9870


![Screenshot from 2021-03-19 17-43-24.png](:/be4700759f434147ace363258c4f7b2f)


</br>

### 12. To stop all processes run following

```
{wsangam@eldy /usr/local/hadoop-3.2.2/sbin }
::./stop-all.sh 
WARNING: Stopping all Apache Hadoop daemons as wsangam in 10 seconds.
WARNING: Use CTRL-C to abort.
Stopping namenodes on [localhost]
Stopping datanodes
Stopping secondary namenodes [eldy]
Stopping nodemanagers
Stopping resourcemanager
```

Using alias
```
{wsangam@eldy ~ }
::hadoopAllStop 
WARNING: Stopping all Apache Hadoop daemons as wsangam in 10 seconds.
WARNING: Use CTRL-C to abort.
Stopping namenodes on [localhost]
Stopping datanodes
Stopping secondary namenodes [eldy]
Stopping nodemanagers
Stopping resourcemanager
```


</br>
</br>
<p style="text-align: center;">*** Thanks ***</p>
