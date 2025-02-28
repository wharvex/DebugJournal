# Debug Journal

## Entry 1

### System details

OS: Windows 11 Professional

Eclipse: "Eclipse IDE for Enterprise Java and Web Developers" Version: 2024-12 (4.34.0)

Java: Adoptium 21

TomEE: 10.0

Oracle DB: 21c XE

### Project name

TomeeDemo3

### Goal

Run a newly-created TomEE Dynamic Web Project (DWP) **_with a JPA facet_** in Eclipse.

It is newly-created in the sense that it does not have any static homepage, it does not have a servlet, it does not even have a main Java package. I completed the DWP creation wizard and then immediately tried to run it.

### Goal achieved?

No.

### Main error

java.lang.NoClassDefFoundError: org/eclipse/persistence/internal/jaxb/WrappedValue

### Expected behavior

A browser window opens, navigating to http://localhost:8080/TomeeDemo3/, which displays a "404" page supplied by TomEE.

### Reason for expecting this behavior

This is the behavior I observed when I ran a newly-created TomEE DWP **_without a JPA facet_** in Eclipse.

### Actual behavior

* Eclipse displays a small "Problem Occurred" popup window mentioning what seems like a task name/description.
* A browser window opens, navigating to http://localhost:8080/TomeeDemo3/, which displays a "local host connection refused" page supplied by Microsoft Edge.
* The console view produces a stack trace.

![eclipse problem occurred popup](https://github.com/wharvex/DebugJournal/blob/main/Entry1/eclipse_problem-occurred.png)

![edge localhost connection refusal page](https://github.com/wharvex/DebugJournal/blob/main/Entry1/edge_refusal.png)

[console output](https://github.com/wharvex/DebugJournal/blob/main/Entry1/console_output.txt)

### Steps to reproduce

1. Open Eclipse
1. File `->` New `->` Dynamic Web Project
1. Wizard -- New Dynamic Web Project
    1. Page -- Dynamic Web Project
        1. Textbox -- Project name
            1. Enter: TomeeDemo3
        1. Section -- Project location
            1. Checkbox -- Use default location
                1. Do: Uncheck
            1. Textbox -- Location
                1. Enter: Some custom filepath
        1. Section -- Target runtime
            1. Dropdown
                1. Select: TomEE (I made this runtime profile earlier using my local installation of TomEE)
        1. Section -- Dynamic web module version (this is one of the facets)
            1. Dropdown
                1. Select: 6.0 (There are several other options, this is the latest one)
        1. Section -- Configuration
            1. Dropdown
                1. Select: Default Configuration for TomEE
            1. Click: Modify `->` "Project Facets" window pops up
            1. Window -- Project Facets
                1. Box -- Project Facet; Version
                    1. Check: Dynamic Web Module; Select: 6.0
                    1. Check: Java; Select: 21
                    1. Check: JavaScript; Select: 1.0
                    1. Check: JPA; Select: 3.1
                    1. _[NEXT STEPS IDEA] JAXB facet is also available here, I did not check it, I could try that_
                1. Box -- Runtimes (select "Runtimes" tab)
                    1. Check: TomEE (only option)
                1. Click: OK `->` "Project Facets" window closes, Configuration dropdown now has `<custom>` selected
        1. Section -- EAR membership
            1. Inaccessible
        1. Section -- Working sets
            1. Checkbox -- Add project to working sets
                1. Do: Uncheck
        1. Click: Next
    1. Page -- Java
        1. Box -- Source folders on build path
            1. Ensure exactly 1 entry for: src\main\java
        1. Textbox -- Default output folder
            1. Enter: src\main\webapp\WEB-INF\classes
        1. Click: Next
    1. Page -- JPA Facet
        1. Section -- Platform
            1. Dropdown
                1. Select: Generic 3.1 (only option)
        1. Section -- JPA implementation
            1. Dropdown -- Type
                1. Select: User Library
                1. _[NEXT STEPS IDEA] "Disable Library Configuration" is also an option, I could try that_
            1. Box
                1. Check: "EclipseLink 4.0.2" (I downloaded this previously with the "Download library..." button)
                1. _[QUESTION] Can I make a user library for OpenJPA, which I've read is the JPA implementation that TomEE is bundled with?_
            1. Checkbox -- Include libraries with this application
                1. Do: Check
                1. _[QUESTION] What exactly does this do?_
        1. Section -- Connection
            1. Dropdown
                1. Select: oracon2 (I made this connection profile previously, it works)
                1. _[NEXT STEPS IDEA] None is also an option, I could try that_
            1. Checkbox -- Add driver library to build path
                1. Do: Uncheck
                    1. I unchecked this because when I check it, I have to choose a driver from a dropdown menu, but the driver I have associated with the oracon2 connection does not appear in that drop-down menu, maybe becuase it is a "Generic JDBC driver".
        1. Section -- Persistent class management
            1. Radio button -- Annotated classes must be listed in persistence.xml
                1. Do: Select
        1. Click: Next
    1. Page -- Web Module
        1. Textbox -- Context root
            1. Enter: TomeeDemo3
        1. Textbox -- Content directory
            1. Enter: src/main/webapp
        1. Checkbox -- Generate web.xml deployment descriptor
            1. Do: Check
        1. Click: Finish
1. The project generates
    1. If there is a problem in the problems view "Downloading external resources is disabled", go to Window `->` Preferences `->` XML (Wild Web Developer) `->` Check "Download external resources like referenced DTD, XSD"
1. Right-click project, then click "Run As", then click "Run on server"

### Questions/Thoughts

1. What is the main error?
    1. java.lang.NoClassDefFoundError: org/eclipse/persistence/internal/jaxb/WrappedValue
1. What is the stack trace?
    1. at java.base/java.lang.ClassLoader.defineClass1(Native Method)
	1. at java.base/java.lang.ClassLoader.defineClass(ClassLoader.java:1027)
	1. at java.base/java.lang.ClassLoader.defineClass(ClassLoader.java:889)
	1. at org.eclipse.persistence.internal.jaxb.JaxbClassLoader.generateClass(JaxbClassLoader.java:127)
        1. [Source](https://github.com/eclipse-ee4j/eclipselink/blob/afb610296a7619794a119050084c3487379136da/moxy/org.eclipse.persistence.moxy/src/main/java/org/eclipse/persistence/internal/jaxb/JaxbClassLoader.java#L127)
	1. at org.eclipse.persistence.jaxb.compiler.MappingsGenerator.generateWrapperClass(MappingsGenerator.java:3295)
	1. at org.eclipse.persistence.jaxb.compiler.MappingsGenerator.generateWrapperClassAndDescriptor(MappingsGenerator.java:3058)
	1. at org.eclipse.persistence.jaxb.compiler.MappingsGenerator.addEnumerationWrapperAndDescriptor(MappingsGenerator.java:3029)
	1. at org.eclipse.persistence.jaxb.compiler.MappingsGenerator.processGlobalElements(MappingsGenerator.java:2976)
	1. at org.eclipse.persistence.jaxb.compiler.MappingsGenerator.generateProject(MappingsGenerator.java:290)
	1. at org.eclipse.persistence.jaxb.compiler.Generator.generateProject(Generator.java:182)
	1. at org.eclipse.persistence.jaxb.JAXBContext$TypeMappingInfoInput.createContextState(JAXBContext.java:1178)
	1. at org.eclipse.persistence.jaxb.JAXBContext$TypeMappingInfoInput.createContextState(JAXBContext.java:1170)
	1. at org.eclipse.persistence.jaxb.JAXBContext.<init>(JAXBContext.java:208)
	1. at org.eclipse.persistence.jaxb.JAXBContextFactory.createContext(JAXBContextFactory.java:167)
	1. at org.eclipse.persistence.jaxb.JAXBContextFactory.createContext(JAXBContextFactory.java:154)
	1. at org.eclipse.persistence.jaxb.JAXBContextFactory.createContext(JAXBContextFactory.java:114)
	1. at org.eclipse.persistence.jaxb.JAXBContextFactory.createContext(JAXBContextFactory.java:104)
	1. at org.eclipse.persistence.jaxb.XMLBindingContextFactory.createContext(XMLBindingContextFactory.java:43)
	1. at jakarta.xml.bind.ContextFinder.find(ContextFinder.java:368)
	1. at jakarta.xml.bind.JAXBContext.newInstance(JAXBContext.java:605)
	1. at jakarta.xml.bind.JAXBContext.newInstance(JAXBContext.java:546)
	1. at org.apache.openejb.jee.JAXBContextFactory.newInstance(JAXBContextFactory.java:93)
	1. at org.apache.openejb.jee.JaxbJavaee.getContext(JaxbJavaee.java:88)
	1. at org.apache.openejb.jee.jpa.unit.JaxbPersistenceFactory.getPersistence(JaxbPersistenceFactory.java:44)
	1. at org.apache.openejb.config.ReadDescriptors.deploy(ReadDescriptors.java:179)
	1. at org.apache.openejb.config.ConfigurationFactory$Chain.deploy(ConfigurationFactory.java:424)
	1. at org.apache.openejb.config.ConfigurationFactory.configureApplication(ConfigurationFactory.java:1037)
	1. at org.apache.tomee.catalina.TomcatWebAppBuilder.startInternal(TomcatWebAppBuilder.java:1315)
	1. at org.apache.tomee.catalina.TomcatWebAppBuilder.configureStart(TomcatWebAppBuilder.java:1159)
	1. at org.apache.tomee.catalina.GlobalListenerSupport.lifecycleEvent(GlobalListenerSupport.java:134)
	1. at org.apache.catalina.util.LifecycleBase.fireLifecycleEvent(LifecycleBase.java:109)
	1. at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:4342)
	1. at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:164)
1. Thoughts on stack trace?
    1. Eclipse's internal JAXB class loader tries (and fails) to load a class called "WrappedValue".
    1. TomEE's OpenEJB uses JAXB to deploy.
    1. Eclipse link is the key.
        1. _[NEXT STEPS IDEA] I could look at that tutorial I found on using TomEE with EclipseLink._
1. What is JAXB?
    1. [Source 1](https://docs.oracle.com/javase/tutorial/jaxb/intro/index.html)
        1. Java Architecture for XML Binding (JAXB) provides a fast and convenient way to bind XML schemas and Java representations, making it easy for Java developers to incorporate XML data and processing functions in Java applications. As part of this process, JAXB provides methods for unmarshalling (reading) XML instance documents into Java content trees, and then marshalling (writing) Java content trees back into XML instance documents. JAXB also provides a way to generate XML schema from Java objects.
1. What are Eclipse project facets?
    1. [Source 1](https://help.eclipse.org/latest/index.jsp?topic=%2Forg.eclipse.jst.j2ee.doc.user%2Ftopics%2Fcfacets.html)
        1. Facets define characteristics and requirements for Java EE projects and are used as part of the runtime configuration.
        1. When you add a facet to a project, that project is configured to perform a certain task, fulfill certain requirements, or have certain characteristics. For example, the EAR facet sets up a project to function as an enterprise application by adding a deployment descriptor and setting up the project's classpath.
        1. You can add facets only to Java EE projects and other types of projects that are based on J2EE projects, such as enterprise application projects, dynamic Web projects, and EJB projects. You cannot add facets to a Java project or plug-in project, for example. Typically, a facet-enabled project has at least one facet when it is created, allowing you to add more facets if necessary. For example, a new EJB project has the EJB Module facet. You can then add other facets to this project like the EJBDoclet (XDoclet) facet. To add a facet to a project, see Adding a facet to a Java EE project.
        1. Some facets require other facets as prerequisites. Other facets cannot be in the same project together. For example, you cannot add the Dynamic Web Module facet to an EJB project because the EJB project already has the EJB Module facet. Some facets can be removed from a project and others cannot.
1. Can I make a user library for OpenJPA, which I've read is the JPA implementation that TomEE is bundled with?
    1. From Window `->` Preferences `->` Java `->` Build Path `->` User Libraries:
        1. User libraries can be added to a Java Build path and bundle a number of external archives.
        1. System libraries will be added to the boot class path when launched.
    1. So it seems like "User Libraries" are just collections of "external" jars. It might therefore be the case that I *wouldn't* need to add a "User Library" for something like OpenJPA, which is "bundled with" OpenEJB, which in turn is bundled with TomEE, so it might not be considered "external", since I don't need to add any of the other J2EE implementations that are included with TomEE as User Libraries.
1. What is EclipseLink?
    1. [Source 1](https://wiki.eclipse.org/EclipseLink/Examples/MOXy/JAXB/SpecifyRuntime)
        1. "You will need to have the JAXB APIs (included in Java SE 6) and eclipselink.jar on your classpath."
        1. "To specify the EclipseLink MOXy (JAXB) runtime should be used you need to add a file called jaxb.properties in the same package as the domain classes with the following entry."
            1. `javax.xml.bind.context.factory=org.eclipse.persistence.jaxb.JAXBContextFactory`
        1. Problem: I don't have any packages yet in my project.
        1. Problem: What does it mean by "domain classes"?
        1. I had been thinking that adding a JAXB facet was not the answer because it was not a pre-requisite for adding the JPA facet. But it just occurred to me that really what might be necessitating it is the fact that I chose EclipseLink as the implementation.
1. Some instructions on setting up OpenJPA to work with a TomEE project are found [here](https://tomee.apache.org/tomee-10.0/docs/openjpa.html).

### Next Steps

1. Make a new project with a JPA facet in all the same ways, except this time in the JPA facet page, in the JPA implementation section, select "Disable Library Configuration".
