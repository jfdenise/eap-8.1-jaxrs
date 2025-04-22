# JBoss EAP 8.1 Beta, evolve a simple Jakarta EE application to use JBoss EAP Provisioning

This application is a Jakarta EE JAX-RS endpoint. 
This project describes the steps to evolve the maven `pom.xml` file to use the EAP Maven Plugin to enable JBOSS EAP provisioning.

We are using the EAP Maven Plugin to:

* Provision a trimmed server and deploy the application.
* Package the server and application as a Bootable JAR.
 
The `pom.xml` file is configured to produce a war to be deployed in JBoss EAP 8.1 server.

## Provisioning the server and the application using the EAP Maven Plugin

Adds the following  plugin configuration to the `plugins` section of the `pom.xml` file:

```
          <plugin>
            <groupId>org.jboss.eap.plugins</groupId>
            <artifactId>eap-maven-plugin</artifactId>
            <version>2.0.0.Beta1-redhat-00011</version>
            <configuration>
              <channels>
                <channel>
                  <!-- The JBoss EAP 8.1 channel that defines the latest server artifacts versions -->
                  <manifest>
                    <groupId>org.jboss.eap.channels</groupId>
                    <artifactId>eap-8.1</artifactId>
                  </manifest>
                </channel>
              </channels>
              <!-- The provisioning configuration. Check the JBoss EAP 8.1 provisioning guide to see the set of
              available feature-packs and layers -->
              <feature-packs>
                <!-- The JBoss EAP 8.1 feature-pack used to provision the JBoss EAP 8.1 server -->
                <feature-pack>
                  <location>org.jboss.eap:wildfly-ee-galleon-pack</location>
                </feature-pack>
              </feature-packs>
              <!-- Layers that bring in server features -->
              <layers>
                <layer>ee-core-profile-server</layer>
              </layers>
              <!-- set 'jboss-fork-embedded' option to 'true' in order to fork Galleon provisioning and CLI scripts execution in dedicated processes -->
              <galleon-options>
                <jboss-fork-embedded>true</jboss-fork-embedded>
              </galleon-options>
            </configuration>
            <executions>
              <execution>
                <goals>
                  <goal>package</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
```

To start the server call: `./target/server/standalone.sh`

### Trimming numbers

#### Full JBoss EAP 8.1 

* Installation size: `290MB`
* Heap Memory: `57MB`

#### Trimmed JBoss EAP 8.1 server

* Installation size: `77MB`
* Heap Memory: `33MB`

## Packaging the server and the application as a Bootable JAR

Adds the following XML configuration to the EAP Maven Plugin configuration:

```
          <bootable-jar>true</bootable-jar>
```

To start the bootable JAR call: `java -jar target/eap-jaxrs-bootable.jar`
