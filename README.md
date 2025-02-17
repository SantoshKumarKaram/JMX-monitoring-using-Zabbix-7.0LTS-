# JMX-monitoring-using-Zabbix-7.0LTS-

=======JMX monitoring using zabbix 7.0LTS=======
--------------
Environment:-
--------------
I have a zabbix server(7.0LTS),zabbix proxy(2),zabbix DB,JMX host(Apache tomcat)--> Centos9

Tomcat server:-
Apache Tomcat instllation:-
https://reintech.io/blog/install-configure-apache-tomcat-centos-9

Now we need to modify the configuration to enable the inbuilt JMX daemon to return its monitoring data.

In the same folder at /opt/tomcat/bin, we create a new file named setenv.sh

sudo nano setenv.sh

CATALINA_OPTS="$CATALINA_OPTS -Dcom.sun.management.jmxremote \
-Dcom.sun.management.jmxremote.port=9000 \
-Dcom.sun.management.jmxremote.authenticate=false \
-Dcom.sun.management.jmxremote.ssl=false \
-Djava.rmi.server.hostname=192.168.47.141"

ls -lh
chmod 750 setenv.sh
sudo chown -R tomcat:tomcat setenv.sh

Restart the tomcat
./catalina.sh stop
./catalina.sh start

ss -ntlp | grep 9000
------------------------------------------------------------------------------------------------------------------
We need to install the Java gateway on our Zabbix server, or Zabbix proxy if the host will be monitored by proxy.


sudo apt install zabbix-java-gateway
The Zabbix Java Gateway has its own configuration file. We can leave the defaults. You can view it using,


sudo nano /etc/zabbix/zabbix_java_gateway.conf
Next to update Zabbix servers (or Zabbix proxy) configuration to point to the Java gateway.


sudo nano /etc/zabbix/zabbix_server.conf
Uncomment and change parameters,


JavaGateway=127.0.0.1
JavaGatewayPort=10052
StartJavaPollers=5
Restart the Zabbix server (or proxy if using the Zabbix proxy)


sudo service zabbix-server restart

------------------------------------------------------------------------------------------------------------------
cd /opt/tomcat/bin/

cd /opt/tomcat/
