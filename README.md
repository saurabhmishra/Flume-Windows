Flume-Windows
=============

Flume tarball for installation on windows

Step 1: Download msysgit for windows from below location

http://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git

Step 2: Clone it locally and downlod the files in c:/tmp/users.

Step 3: Unzip the file into hadoop installation directory for example: "C:/hdp/hadoop/"

Step 4: Create the configuration file at C:\hdp\hadoop\apache-flume-1.3.1-bin\conf as below for example:

Source is syslog, channel is memory and sink is HDFS
----------------------
syslog-agent.sources = Syslog
syslog-agent.channels = MemoryChannel-1 
syslog-agent.sinks = HDFS-LAB

Source configuration
----------------------
syslog-agent.sources.Syslog.type = syslogTcp
syslog-agent.sources.Syslog.port = 5140

channel configuration
----------------------
syslog-agent.sources.Syslog.channels = MemoryChannel-1 
syslog-agent.channels.MemoryChannel-1.type = memory
syslog-agent.sinks.HDFS-LAB.channel = MemoryChannel-1

sink configuration
----------------------
syslog-agent.sinks.HDFS-LAB.type = hdfs 
syslog-agent.sinks.HDFS-LAB.hdfs.path = hdfs://master-1:8020/apps/flume/syslogs/
syslog-agent.sinks.HDFS-LAB.hdfs.file.Prefix = syslogfiles
syslog-agent.sinks.HDFS-LAB.hdfs.file.rollInterval = 60
syslog-agent.sinks.HDFS-LAB.hdfs.file.Type = SequenceFile

Step 4: Start flume agent to listen to syslog source 

cd C:\hdp\hadoop\apache-flume-1.3.1-bin\conf

java -Xmx20m -Dlog4j.configuration=log4j.properties -cp "C:\hdp\hadoop\apache-flume-1.3.1-bin\lib\*" org.apache.flume.node.Application -f C:\hdp\hadoop\apache-flume-1.3.1-bin\conf\flume.conf -n syslog-agent
 
Step 5: Test it by initiating netcat (http://eternallybored.org/misc/netcat/netcat-win32-1.12.zip) on the master node

   echo "Starting a syslog message" > /tmp/foo

   nc -v master-1 5140 < /tmp/foo

Step 6: Verify If you get a file in hdfs location /apps/flume/syslogs/

Software requirement:

1- Hadoop for windows (http://hortonworks.com/thankyou-hdp11-win/)

2- netcat (http://eternallybored.org/misc/netcat/netcat-win32-1.12.zip)

3- msysgit (http://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git)

4- Java (jdk) 1.6.0_31
