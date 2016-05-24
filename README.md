# WildflySampleWebApp

To use:

* Download Wildfly 10
* Add the following XML configuration to standalone/configuration/standalone.xml 

	      <subsystem xmlns="urn:jboss:domain:naming:2.0">
            <bindings>
                <object-factory name="java:global/LocalMongoClient" module="org.mongodb" class="com.mongodb.client.jndi.MongoClientFactory">
                    <environment>
                        <property name="connectionString" value="mongodb://localhost:27017"/>
                    </environment>
                </object-factory>
            </bindings>
            <remote-naming/>
        </subsystem>
* Create a new module at modules/system/layers/base/org/mongodb/main.  Add this module.xml file to that directory

        <module xmlns="urn:jboss:module:1.3" name="org.mongodb">
           <resources>
               <resource-root path="mongo-java-driver-3.3.0-SNAPSHOT.jar"/>
           </resources>
           <dependencies>
               <module name="javax.api"/>
               <module name="javax.transaction.api"/>
               <module name="javax.servlet.api" optional="true"/>
           </dependencies>
        </module>

* Also copy mongo-java-driver-3.3.x.jar to modules/system/layers/base/org/mongodb/main directory. 
* Start Wildfly
* Start mongod and create a single document in the test.test namespace with a numeric _id value
* Deploy the web app into Wildfly (you can use the admin console)
* Go the the URI /WildflySampleWebApp/HelloMongoFromJNDI in a web browser, and it should show a page with that _id value if everything is working correctly.
