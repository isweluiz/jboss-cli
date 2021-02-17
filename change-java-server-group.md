# How do I set different version of JDK in JBoss EAP 7 / 6 domain mode?


### Environment
Red Hat JBoss Enterprise Application Platform (EAP)
- 7.x
- 6.x

### Issue
- Is it possible to set different versions of JDK to different server-groups/server-instances or profiles. For example, one profile needs to use JDK 1.7 and another one JDK 1.6 ?
- Running two different JDK for servers in parallel on same host slave.
- Does EAP6 support use of JVM option in host.xml something like <option value="-version:1.7*"/>?
- Configure a new server group with a different JVM than other server groups

### Resolution
- This feature is fully tested and supported on EAP 6.2. The code is present in EAP 6.1.1 and is very likely to work there, but has not gone through full QA testing so it isn't officially supported.

- Just put the property java-home at the jvm line in each server-group level at the domain.xml file (as shown below) or in each server level in the host.xml file:

```xml
        <server-group name="main-server-group" profile="full">
            <jvm name="default" java-home="/path/jdk">
                <heap size="128m" max-size="1024m"/>
            </jvm>
            <socket-binding-group ref="full-sockets"/>
        </server-group>
 ```
