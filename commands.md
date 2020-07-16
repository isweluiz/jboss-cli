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





### For new instances location, copy this folders
* configuration
* deployments
* lib


**By:** Luiz Eduardo

> [!NOTE]
> Visit my [blog](http://blog.isweluiz.com.br) 