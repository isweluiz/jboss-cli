
# Start and Stop instances JbossEap in a Action on zabbix

## Melhorias providas para o ambiente de Middleware
- Monitorar estado das Instâncias
   - - Identificar instância travada/parada dos HC's
- Tomar uma ação(action) após identificar instância travada ou parada.


### Start Instances Jboss
#### Action
- Nome da trigger contêm Down!
- Host igual **servera**


#### Operation
- Script personalizado
- Zabbix Agent

Script: 
```bash
export JAVA_HOME=/opt/java/ && /opt/jboss-eap/bin/jboss-cli.sh -c controller=master --user=admin --password=JBoss@RedHat123 --commands="/host={HOST.NAME}/server-config={ITEM.NAME}:start"
```

### Stop Instances Jboss
#### Action
 - Nome da trigger contêm travada!
 - Host igual **servera**
#### Operation
- Script personalizado
- Zabbix Agent


Script: 
```bash
for i in $( ps -aux | grep -i {ITEM.NAME} | grep -v grep | sed s/'     '/' '/g | awk '{print $2}' ) ; do sudo kill -9 $i ; done
```

