# Karaf Camel and JSON logging for EFK

This example demonstrates how you can use Apache Camel with Karaf and log in JSON for EFK

### Code generation
Project is generated with the command:

    mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate \
      -DarchetypeCatalog=https://maven.repository.redhat.com/earlyaccess/all/io/fabric8/archetypes/archetypes-catalog/2.2.0.fuse-000092-redhat-2/archetypes-catalog-2.2.0.fuse-000092-redhat-2-archetype-catalog.xml \
      -DarchetypeGroupId=org.jboss.fuse.fis.archetypes \
      -DarchetypeArtifactId=karaf-camel-log-archetype \
      -DarchetypeVersion=2.2.0.fuse-000092-redhat-2


### Configuration
Change the start level from 30 to 8 for the following bundles in startup.properties 

    reference\:file\:com/fasterxml/jackson/core/jackson-annotations/2.8.11/jackson-annotations-2.8.11.jar = 8
    reference\:file\:com/fasterxml/jackson/core/jackson-core/2.8.11/jackson-core-2.8.11.jar = 8
    reference\:file\:com/fasterxml/jackson/core/jackson-databind/2.8.11.1/jackson-databind-2.8.11.1.jar = 8

In org.ops4j.pax.logging.cfg file, change the logging configuration from 

    log4j2.appender.console.layout.type = PatternLayout
    log4j2.appender.console.layout.pattern = %d{DEFAULT} | %-5.5p | %-20.20t | %-32.32c{1.} | %X{bundle.id} - %X{bundle.name} - %X{bundle.version} | %m%n
    log4j2.appender.console.layout.pattern = %d{ABSOLUTE} %-5.5p {%t} [%C.%M()] (%F:%L) : %m%n

to

    log4j2.appender.console.layout.type = JsonLayout

### Building
The example can be built with

    mvn clean install    
    mvn fabric8:deploy
    oc get pods
    oc logs <name of pod>

### Run locally
    target/assembly/bin/fuse

    tail -f target/assembly/data/log/karaf.log

 
