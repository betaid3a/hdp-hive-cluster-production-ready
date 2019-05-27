# Tips for taming the big elephant [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

A curated list of tips about Hortonworks Data Platform (HDP) 2.6.5 for production environments.

# Content
Choose the one you are interested in and jump directly to the right area. 

- [How to setup a JDBC SQL client tool, like DbVisualizer, to access HDP Hive with zookeeper and SSL enabled](#how-to-setup-a-jdbc-sql-client-tool-like-dbvisualizer-to-access-hdp-hive-with-zookeeper-and-ssl-enabled)

___

# How to setup a JDBC SQL client tool, like DbVisualizer, to access HDP Hive with zookeeper and SSL enabled 	

### Introduction
This guide is particular to DBVisualizer 10.0.13 and Hortonwork HiveServer2 from HDP 2.6.5 (Hive 1.2.1000.2.6.5.0-292).
It may work with other versions of DBVisualizer. Other versions of Hive will obviously require newer or older .jar files.

###  Get the right jar Files
Possible jars needed (it may vary from version to version):
* hadoop-auth.jar
* hadoop-common.jar
* hive-jdbc-<hive-version>-standalone.jar

Go to the host machine where HiveServer2 is running. 
Check which version of hive you're running:
```
hive --version
Hive 1.2.1000.2.6.5.0-292
```
Locate the needed jars with locate command and download them to your host:
```
locate *hadoop-auth.jar
locate *hadoop-common.jar
locate *hive-jdbc*-standalone.jar
```
There will be 2 diferent versions for the hive-jdbc-standalone jar, copy the one that matches your hive version.
After collecting the right jars go to DBVizualizer.

###  Create a new custom Driver in DBVisualizer
Open DBVisualizer and Select Tools > Driver Manager at top.
In the Driver Manager pane, click the “Create a new driver” icon > Now click the folder icon on the right side and navigate to the folder where you placed the .jar files. Select all the files at once and click OK.

![Driver Manager](/pics/jdbc-client/driver-manager.png)

###  Import certificates into DbVisualizer JVM truststore
Check the Java Home of DbVisualizer. Select Tools > Driver Manager at top.
![Java Home](/pics/jdbc-client/java-home.png)

Then add certificates into the truststore with keytool

Launch the Command Prompt with administrator privileges. Browse to DbVisualizer/jre/bin folder and run keytool to import the needed certificates into the DbVisualizer truststore :
```
cd C:\Program Files\DbVisualizer\jre\bin
keytool.exe -import -alias UNICRE-CA -file "C:\Tools\certs\UNICRE-CA.cer -keystore "C:\Program Files\DbVisualizer\jre\lib\security\cacerts"
keytool.exe -import -alias UNICRE-SubCA -file "C:\Tools\certs\UNICRE-SubCA.cer -keystore "C:\Program Files\DbVisualizer\jre\lib\security\cacerts"
```
Default password for the truststore is : changeit

###  Create a new hive-hadoop connection using the new driver
Open DBVisualizer and Select Tools > Connection Wizard at top.
Enter the connection alias for the new database connection and select the recently created driver
Choose Database URL from Settings Format and the Database URL and your Database Userid
Also make sure to follow the screenshot bellow

ENJOY DbVizualizer for Hive just like you enjoy SQL Developer for Oracle!



