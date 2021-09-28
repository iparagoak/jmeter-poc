Edit the jmeter.properties (apache-jmeter.xxxx/bin directory)

Master Jmeter machine should have ips of all slave -
remote_hosts= <Slave machine's ip > (comma seprated for multiple ip's) 
On Slave machine's ip of master machine need to be used.
remote_hosts= <Master machine ip>

Create folders if not present : mkdir -p JMeter_reports && mkdir -p JMeter_reports/HTMLReport

Run slave command(s) before running test plan from master : .<jmeter_path>/bin/jmeter-server -Jserver.rmi.ssl.disable=true

Use this command to run the plan : .<jmeter_path>/bin/jmeter -n -t "<JMETER_TEST_PLAN_PATH>.jmx" -l <JMeter_reports_path>/JMeter_reports/HTMLReport/<CSV_NAME>.csv  -e -o <JMeter_reports_path>/JMeter_reports/HTMLReport/HTML/ -r -Jserver.rmi.ssl.disable=true

Then create a zip file of the report to download : zip -r <ZIP_NAME>.zip JMeter_reports/ && sudo rm -rf JMeter_reports/ && mkdir -p JMeter_reports && mkdir -p JMeter_reports/HTMLReport



Example : 

slave : ./chinna-jmeter-5.3/bin/jmeter-server -Jserver.rmi.ssl.disable=true

master : ./chinna-jmeter-5.3/bin/jmeter -n -t "/home/ec2-user/Jmeter_TestPlan.jmx" -l /home/ec2-user/JMeter_reports/HTMLReport/Jmeter_TestReport.csv  -e -o /home/ec2-user/JMeter_reports/HTMLReport/HTML/ -r -Jserver.rmi.ssl.disable=true

compression : zip -r Jmeter_TestReport.zip JMeter_reports/ && sudo rm -rf JMeter_reports/ && mkdir -p JMeter_reports && mkdir -p JMeter_reports/HTMLReport



Installed Jmeter on each Master and Slave (Copied Binary file of apache-jmeter-xxx)

Installed PerfMon Plugin using  below steps -

Login to instance and sudo as root if jmeter path is ====> chinna-jmeter-5.3


Step by Step commands : 
1. wget http://search.maven.org/remotecontent?filepath=kg/apc/cmdrunner/2.2/cmdrunner-2.2.jar

2. cp remotecontent?filepath=kg%2Fapc%2Fcmdrunner%2F2.2%2Fcmdrunner-2.2.jar <JMETER-PATH>/lib/cmdrunner-2.2.jar

3. wget https://repo1.maven.org/maven2/kg/apc/jmeter-plugins-manager/1.6/jmeter-plugins-manager-1.6.jar

3. cp jmeter-plugins-manager-1.6.jar <JMETER-PATH>/lib/ext/

4. cp jmeter-plugins-manager-1.6.jar /lib/ext/

5. java -cp <JMETER-PATH>/lib/ext/jmeter-plugins-manager-1.6.jar org.jmeterplugins.repository.PluginManagerCMDInstaller

6. ./chinna-jmeter-5.3/bin/PluginsManagerCMD.sh status

7. ./chinna-jmeter-5.3/bin/PluginsManagerCMD.sh install jpgc-perfmon=2.1


single line command (above all 1 - 7 commands ): 
wget http://search.maven.org/remotecontent?filepath=kg/apc/cmdrunner/2.2/cmdrunner-2.2.jar && cp remotecontent?filepath=kg%2Fapc%2Fcmdrunner%2F2.2%2Fcmdrunner-2.2.jar chinna-jmeter-5.3/lib/cmdrunner-2.2.jar && wget https://repo1.maven.org/maven2/kg/apc/jmeter-plugins-manager/1.6/jmeter-plugins-manager-1.6.jar && cp jmeter-plugins-manager-1.6.jar chinna-jmeter-5.3/lib/ext/ && java -cp chinna-jmeter-5.3/lib/ext/jmeter-plugins-manager-1.6.jar org.jmeterplugins.repository.PluginManagerCMDInstaller && ./chinna-jmeter-5.3/bin/PluginsManagerCMD.sh status && ./chinna-jmeter-5.3/bin/PluginsManagerCMD.sh install jpgc-perfmon=2.1


On application server (s) : 

wget https://github.com/undera/perfmon-agent/releases/download/2.2.3/ServerAgent-2.2.3.zip
uzip ServerAgent-2.2.3.zip
sudo ./ServerAgent-2.2.3/startAgent.sh

