
# Start and Stop instances JbossEap in a Action on zabbix


### Start Instances Jboss
- Action
 - - Nome da trigger contêm Down!
 - - 	Host igual **servera**
- Operation

```bash
export JAVA_HOME=/usr/java/jdk1.8.0_111 && /opt/jboss-eap-6.4/bin/jboss-cli.sh -c controller=master --user=admin --password=JBoss@RedHat123 --commands="/host={HOST.NAME}/server-config={ITEM.NAME}:start"
```

### Stop Instances Jboss
- Action
- - 	Nome da trigger contêm travada!
- - 	Host igual **servera**
- Operation

```bash
for i in $( ps -aux | grep -i {ITEM.NAME} | grep -v grep | sed s/'     '/' '/g | awk '{print $2}' ) ; do sudo kill -9 $i ; done
```

