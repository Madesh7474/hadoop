Step 1 : Install java jdk 8
-> sudo apt install openjdk-8-jdk

Step 2 : Open bashrc and add configuration files
sudo nano .bashrc

add these:
-> export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin 
export HADOOP_HOME=~/hadoop-3.2.3/ 
export PATH=$PATH:$HADOOP_HOME/bin 
export PATH=$PATH:$HADOOP_HOME/sbin 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME 
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native" 
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.2.3.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs 
export PDSH_RCMD_TYPE=ssh

Step 3: Install ssh
-> sudo apt-get install ssh

Step 4: Download hadoop-3.2.3.tar.gz from browser

Step 5: Install hadoop
-> tar -zxvf ~/Downloads/hadoop-3.2.3.tar.gz 

Step 6: Go to hadoop location
-> cd hadoop-3.2.3/etc/hadoop

Step 7: now open hadoop-env.sh
-> sudo nano hadoop-env.h

Step 8: In hadoop-env.sh find '#JAVA HOME', remove the '#' and replace that specific line with this
-> JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

Step 9: Exit the hadoop-env.sh and open core-site.xml, hdfs-site.xml, mapred-site.xml and yarn-site.xml and add their respective config give below
-> sudo nano core-site.xml
-> <configuration> 
 <property> 
 <name>fs.defaultFS</name> 
 <value>hdfs://localhost:9000</value>  </property> 
 <property> 
<name>hadoop.proxyuser.dataflair.groups</name> <value>*</value> 
 </property> 
 <property> 
<name>hadoop.proxyuser.dataflair.hosts</name> <value>*</value> 
 </property> 
 <property> 
<name>hadoop.proxyuser.server.hosts</name> <value>*</value> 
 </property> 
 <property> 
<name>hadoop.proxyuser.server.groups</name> <value>*</value> 
 </property> 
</configuration>

-> sudo nano hdfs-site.xml
-> <configuration> 
 <property> 
 <name>dfs.replication</name> 
 <value>1</value> 
 </property> 
</configuration>

-> sudo nano mapred-site.xml
-> <configuration> 
 <property> 
 <name>mapreduce.framework.name</name>  <value>yarn</value> 
 </property> 
 <property>
 <name>mapreduce.application.classpath</name> 
<value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value> 
 </property> 
</configuration>

-> sudo nano yarn-site.xml
-> <configuration> 
 <property> 
 <name>yarn.nodemanager.aux-services</name> 
 <value>mapreduce_shuffle</value> 
 </property> 
 <property> 
 <name>yarn.nodemanager.env-whitelist</name> 
<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREP END_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value> 
 </property> 
</configuration>

Step 10: excute these commands in terminal 
-> ssh localhost
-> ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
-> chmod 0600 ~/.ssh/authorized_keys
-> hadoop-3.2.3/bin/hdfs namenode -format
-> export PDSH_RCMD_TYPE=ssh
-> start-all.sh(to start hadoop)
-> stop-all.sh(to stop hadoop)
