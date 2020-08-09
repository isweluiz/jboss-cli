# Jboss CLI

### This file contain my anotations about jboss commands

> [!NOTE]
> Visit my [blog](http://blog.isweluiz.com.br) 


## Standalone mode

### Running the standalone.sh script to start the EAP server:
>`$ ./standalone.sh -Djboss.server.base.dir=/path/to/base/directory -Djboss.home.dir=/path/to/home/directory`


### The port-offset attribute
> `$ ./standalone.sh -Djboss.socket.binding.port-offset=10000`

### For a new address management 
> `$ ./standalone.sh -bmanagement 127.0.0.1`


### To connect to a different host machine
> `$JBOSS_HOME/bin/jboss-cli.sh --connect --controller=ip:port`

### Command external execution
> `$JBOSS_HOME/jboss-cli.sh -c --controller=localhost:9990 --command="/subsystem=datasources:read-resource" `

### Read-resources of the public interface
> `/interface=public:read-resource`

### Read all configuration of the undertow
>`/subsystem=undertow/configuration=*:read-resource`

### Read all configuration of the system
> `/:read-resource(recursive=true)`
`:read-resource(recursive=true) > /tmp/output.txt`

### Data source access
> `[standalone@localhost:9990 /] cd /subsystem=datasources/data-source=ExampleDS`
`[standalone@localhost:9990 data-source=ExampleDS] :write-attribute(name=min-pool-size,value=5)`
{"outcome" => "success"}

### CLI add users and management users
> `Jboss root > /opt/jboss-eap/bin/add-user.sh -u "luiz.eduardo" -p 'JB321@' -g 'dev'`

### RBAC authorization
>`[standalone@localhost:9990 access=authorization] cd /core-service=management/access=authorization`
`[standalone@localhost:9990 access=authorization] :write-attribute(name=provider, value=rbac)`

> `[standalone@localhost:9990 access=authorization] :write-attribute(name=provider, value=simple)`

### Domain controller start
> `$ ./domain.sh -Djboss.bind.address.management=192.168.0.14`

### Connect to the domain
>`$ ./domain.sh -Djboss.bind.address.management=192.168.0.15 -Djboss.domain.master.address=192.168.0.14`

> `$ ./domain.sh -Djboss.domain.base.dir=/usr/local/eap/configuration --domain-config=mydomain.xml --host-config=myhost.xml`

> `$./domain.sh -Djboss.domain.base.dir=/usr/local/eap/configuration`

### Configuration File Backups and Snapshots
> `[domain@localhost:9999 /]  /host=master:take-snapshot`

> `[standalone@localhost:9990 /] :take-snapshot `

```json
{
    "outcome" => "success",
    "result" => "/opt/jboss-eap/standalone/configuration/standalone_xml_history/snapshot/20200404-114515685standalone.xml"
}
```
> `[standalone@localhost:9990 /] :list-snapshots`

```json
{
    "outcome" => "success",
    "result" => {
        "directory" => "/opt/jboss-eap/standalone/configuration/standalone_xml_history/snapshot",
        "names" => [
            "20200404-114515685standalone.xml",
            "20200404-114609110standalone.xml"
        ]
    }
}
```
> `[standalone@localhost:9990 /] :delete-snapshot(name=20200404-114609110standalone.xml)`
{"outcome" => "success"


### Handling server start/stop/restart/reload

> `/server-group=main-server-group:start-servers`

> `/server-group=main-server-group:restart-servers`

>`/server-group=main-server-group:stop-servers`

>`/server-group=main-server-group:reload-servers`


### A practical script for shutting down a domain
>`./jboss-cli.sh --connect controller=192.168.10.1:9990 /host=slave:shutdown`

>`./jboss-cli.sh --connect controller=192.168.10.1:9990 /host=master:shutdown`

### How to un-assign an application from a Server Group
>`/server-group=main-server-group/deployment=web-demo.war:remove`

### How to remove an application from the Repository
> `/deployment=web-demo.war:remove`

# Deploy
### A practical bash script for deploying one application
> `./jboss-cli.sh --connect controller=192.168.10.1:9990 --commands="deploy $1  --server-groups=main-server-group"`

### And to undeploy the application
 >`./jboss-cli.sh --connect controller=192.168.10.1:9990 --commands="undeploy $1  --all-relevant-server-groups"`

### Find out the location where the application is stored,
> `[standalone@localhost:9990 /] deployment=demoweb.war:read-resource `

```json
{
  "outcome" => "success",
  "result" => {
  "content" => [{"hash" => bytes {
  0x56, 0x3b, 0x77, 0x4b, 0x70, 0x0b, 0xb3, 0xe5,
  0x1e, 0x2b, 0x4c, 0x6b, 0xdb, 0xef, 0x08, 0xb7,
  0xfc, 0xaf, 0xc6, 0x22
  }}],
  "disabled-time" => 1483561413911L,
  "disabled-timestamp" => "2017-01-04 21:23:33,911 CET",
  "enabled" => false,
  "enabled-time" => 1483561285412L,
  "enabled-timestamp" => "2017-01-04 21:21:25,412 CET",
  "name" => "demoweb.war",
  "owner" => [
  ("subsystem" => "deployment-scanner"),
  ("scanner" => "default")
  ],
  "persistent" => false,
  "runtime-name" => "demoweb.war",
  "subdeployment" => undefined,
  "subsystem" => undefined
  }
}
```

### Explode command - , the application must be disabled in order to do that
>`[standalone@localhost:9990 /] /deployment=demoweb.war:explode()`

### Deploying multiple versions of one application
> `[standalone@localhost:9990 /] deploy /home/francesco/mavenprojects/basicweb/webapp.war --name=webapp1.war`

>`[standalone@localhost:9990 /] deploy /home/francesco/mavenprojects/basicweb/webapp.war --name=webapp2.war`

> [!IMPORTANT]
>  Notice we are using the same runtime name, but we made only one version enabled. Enabling both
application with the same runtime name will eventually cause a conflict

### Deploying in Domain mode
> ` [domain@localhost:9990 /] deploy --name=webapp.war --server-groups=main-server-group`

> `[domain@localhost:9990 /] deploy application.war --all-server-groups`

> `[domain@localhost:9990 /] deploy /home/jboss/projects/webapp.war --disabled`

> `[domain@localhost:9990 /] deployment-info --name=webapp.war`
```json
NAME RUNTIME-NAME
webapp.war webapp.war
SERVER-GROUP STATE
main-server-group not added
other-server-group not added
```


## CLI Useful Commands

- jboss Jvm OS Name:
> `/core-service=platform-mbean/type=operating-system:read-attribute(name=name)`

- jboss Jvm OS Version:
> `/core-service=platform-mbean/type=operating-system:read-attribute(name=version)`

- Status
read-attribute(name=server-state)
read-attribute(name=status)

#### Mbean – Complete Resource
> `/core-service=platform-mbean/type=memory:read-resource(include-runtime=true)`
> `/core-service=platform-mbean/type=threading:read-resource(include-runtime=true)`
> `/core-service=platform-mbean/type=memory-manager:read-resource(include-runtime=true)`
> `/core-service=platform-mbean/type=memory-pool:read-resource(include-runtime=true)`
> `/core-service=platform-mbean/type=garbage-collector:read-resource(include-runtime=true)`
> `/core-service=platform-mbean/type=operating-system:read-resource(include-runtime=true)`

### Memory
> `/core-service=platform-mbean/type=memory:read-attribute(name=heap-memory-usage)`
> `/core-service=platform-mbean/type=memory:read-attribute(name=heap-memory-usage)`
> `/core-service=platform-mbean/type=memory:read-attribute(name=heap-memory-usage)`
> `/core-service=platform-mbean/type=memory:read-attribute(name=non-heap-memory-usage)`
> `/core-service=platform-mbean/type=memory:read-attribute(name=non-heap-memory-usage)`
> `/core-service=platform-mbean/type=memory:read-attribute(name=non-heap-memory-usage)`

### GC
> `/core-service=platform-mbean/type=garbage-collector/name=PS_MarkSweep:read-attribute(name=collection-count)`
> `/core-service=platform-mbean/type=garbage-collector/name=PS_MarkSweep:read-attribute(name=collection-time)`
> `/core-service=platform-mbean/type=garbage-collector/name=PS_Scavenge:read-attribute(name=collection-count)`
> `/core-service=platform-mbean/type=garbage-collector/name=PS_Scavenge:read-attribute(name=collection-time)`

### Threads
> `/core-service=platform-mbean/type=threading:read-attribute(name=thread-count)`
> `/core-service=platform-mbean/type=threading:read-attribute(name=daemon-thread-count)`
> `/core-service=platform-mbean/type=threading:read-attribute(name=current-thread-cpu-time)`

### Datasources
- Jboss DataSource Pool Active Count :
> `/subsystem=datasources/data-source=%1$s/statistics=pool:read-attribute(name=ActiveCount)`

- Jboss DataSource Pool Size:
> `/subsystem=datasources/data-source=%1$s:read-attribute(name=max-pool-size)`

- Jboss Datasource Max Pool Size:
> `/subsystem=datasources/data-source=%1$s:read-attribute(name=max-pool-size)`

- Jboss Datasource Min Pool Size:
> `/subsystem=datasources/data-source=%1$s:read-attribute(name=min-pool-size)`

> `/subsystem=datasources/data-source=SimulatorDS/statistics=pool`
> `/subsystem=datasources/data-source=SimulatorDS/statistics=pool`

> `/subsystem=datasources/data-source=DataSource/statistics=pool`
`ATTR_NAME= "AvailableCount"`

>`/subsystem=datasources/data-source=DataSource/statistics=pool`
`ATTR_NAME= "MaxUsedCount"`

### Deployments
- Jboss Application Status:
> `/deployment=%3$s:read-attribute(name=status)`

- Jboss Application Active Session count:
> `deployment=%1$s/subsystem=web:read-attribute(name=active-sessions)!sum(/:host,/:server)`

- Hits
- JVM Hits:
> `/server=Servername/subsystem=web/connector=http:read-attribute(name=requestCount)`



```json
[domain@192.168.99.110:9990 subsystem=messaging-activemq] pwd
/host=servera/server=serverA.1/subsystem=messaging-activemq
[domain@192.168.99.110:9990 subsystem=messaging-activemq] ./server=default/cluster-connection=my-cluster:read-attribute(name=topology)
{
    "outcome" => "success",
    "result" => "topology on Topology@1523fb89[owner=ClusterConnectionImpl@1091634163[nodeUUID=242e1272-cc15-11ea-a314-d124d7a0dc22, connector=TransportConfiguration(name=http-connector, factory=org-apache-activemq-artemis-core-remoting-impl-netty-NettyConnectorFactory) ?httpUpgradeEnabled=true&httpPpgradeEndpoint=http-acceptor&port=8080&host=192-168-99-107, address=jms, server=ActiveMQServerImpl::serverUUID=242e1272-cc15-11ea-a314-d124d7a0dc22]]:
    298939e2-cc15-11ea-871b-570c3505ecec => TopologyMember[id = 298939e2-cc15-11ea-871b-570c3505ecec, connector=Pair[a=TransportConfiguration(name=http-connector, factory=org-apache-activemq-artemis-core-remoting-impl-netty-NettyConnectorFactory) ?httpUpgradeEnabled=true&httpPpgradeEndpoint=http-acceptor&port=8230&host=192-168-99-107, b=null], backupGroupName=null, scaleDownGroupName=null]
    2a0520c6-cc15-11ea-8997-6b6673c58c29 => TopologyMember[id = 2a0520c6-cc15-11ea-8997-6b6673c58c29, connector=Pair[a=TransportConfiguration(name=http-connector, factory=org-apache-activemq-artemis-core-remoting-impl-netty-NettyConnectorFactory) ?httpUpgradeEnabled=true&httpPpgradeEndpoint=http-acceptor&port=8230&host=192-168-99-108, b=null], backupGroupName=null, scaleDownGroupName=null]
    242e1272-cc15-11ea-a314-d124d7a0dc22 => TopologyMember[id = 242e1272-cc15-11ea-a314-d124d7a0dc22, connector=Pair[a=TransportConfiguration(name=http-connector, factory=org-apache-activemq-artemis-core-remoting-impl-netty-NettyConnectorFactory) ?httpUpgradeEnabled=true&httpPpgradeEndpoint=http-acceptor&port=8080&host=192-168-99-107, b=null], backupGroupName=null, scaleDownGroupName=null]
    256d703b-cc15-11ea-94a7-698f306ed1b7 => TopologyMember[id = 256d703b-cc15-11ea-94a7-698f306ed1b7, connector=Pair[a=TransportConfiguration(name=http-connector, factory=org-apache-activemq-artemis-core-remoting-impl-netty-NettyConnectorFactory) ?httpUpgradeEnabled=true&httpPpgradeEndpoint=http-acceptor&port=8080&host=192-168-99-108, b=null], backupGroupName=null, scaleDownGroupName=null]
    nodes=4    members=4"
}
[domain@192.168.99.110:9990 subsystem=messaging-activemq
```
] 

### Ativar/Remover autenticação local

>`[standalone@locahost:10990 /] /core-service=management/\security-realm=ManagementRealm/authentication=local:remove()`

### Para remover a autenticação local do servidor standalone, execute o seguinte comando:

> `[domain@172.25.250.254:9990 /]/host=master/core-service=management\/security-realm=ManagementRealm/authentication=local:remove()`

### For new instances location, copy this folders
* configuration
* deployments
* lib


**By:** Luiz Eduardo

> [!NOTE]
> Visit my [blog](http://blog.isweluiz.com.br) 