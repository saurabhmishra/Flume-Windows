Flume-Windows
=============

Flume tarball for installation on windows

Step 1: Download msysgit for windows from below location(except all defaults)

         http://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git

Step 2: Clone existing repository it locally using GUI and downlod the files in c:/tmp/users.

Or 

Step 1: Download the zip file for entire repo https://github.com/saurabhmishra/Flume-Windows/archive/master.zip 
   
Step 2: Unzip it to extract apache-flume-1.3.1-bin.zip into hadoop installation directory

Step 3: Unzip the file into hadoop installation directory for example: "C:/hdp/hadoop/"

Step 4: Create the configuration file at C:\hdp\hadoop\apache-flume-1.3.1-bin\conf as below for example , Source is syslog, channel is memory and sink is HDFS

            syslog-agent.sources = SyslogNetCat
            
            syslog-agent.channels = MemoryChannel-1 
            
            syslog-agent.sinks = HDFS-LAB
            
            syslog-agent.sources.SyslogNetCat.type = syslogTcp
            
            syslog-agent.sources.SyslogNetCat.port = 5140
            
            syslog-agent.sources.SyslogNetCat.channels = MemoryChannel-1 
            
            syslog-agent.channels.MemoryChannel-1.type = memory
            
            syslog-agent.sinks.HDFS-LAB.channel = MemoryChannel-1
            
            syslog-agent.sinks.HDFS-LAB.type = hdfs 
            
            syslog-agent.sinks.HDFS-LAB.hdfs.path = hdfs://master-1:8020/apps/flume/syslogs/%y-%m-%d/%H%M/%S
            
            syslog-agent.sinks.HDFS-LAB.hdfs.filePrefix = syslogfiles
            
            syslog-agent.sinks.HDFS-LAB.hdfs.filerollInterval = 120
            
            syslog-agent.sinks.HDFS-LAB.hdfs.fileType = SequenceFile
            

Step 5: Start flume agent to listen to syslog source 

         cd C:\hdp\hadoop\apache-flume-1.3.1-bin\conf

         java -Xmx20m -Dlog4j.configuration=log4j.properties -cp "C:\hdp\hadoop\apache-flume-1.3.1-bin\lib\*" org.apache.flume.node.Application -f C:\hdp\hadoop\apache-flume-1.3.1-bin\conf\flume.conf -n syslog-agent
 
Step 6: Test it by initiating netcat (http://eternallybored.org/misc/netcat/netcat-win32-1.12.zip) on the master node

         echo "Starting a syslog message" > /tmp/foo
         
         nc -v master-1 5140 < /tmp/foo

Step 7: Verify If you get a file in hdfs location /apps/flume/syslogs/

Software requirement:
------------------------
1- Hadoop for windows (http://hortonworks.com/thankyou-hdp11-win/)

2- netcat (http://eternallybored.org/misc/netcat/netcat-win32-1.12.zip)

3- msysgit (http://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git)

4- Java (jdk) 1.6.0_31

Reference for different sink and source Properties
-------------------------
Flume User Guide : http://flume.apache.org/FlumeUserGuide.html
