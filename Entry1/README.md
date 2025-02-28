# Debug journal

## Entry 1

### Project name

TomeeDemo3

### Goal

Run a newly-created (i.e. homepage-less) TomEE Dynamic Web Project (DWP) **_with a JPA facet_** in Eclipse.

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

1. Open "Eclipse IDE for Enterprise Java and Web Developers" Version: 2024-12 (4.34.0)
1. File -> New -> Dynamic Web Project
1. DWP creation wizard
    1. Enter project name
    1. Choose project location
    1. Choose Target runtime
        1. TomEE
    1. Modify default TomEE config to add JPA facet
        1. JAXB facet is also available here, I did not choose it, but maybe I should have?
    1. Click Next twice
    1. JPA Facet page
        1. Platform: Generic 3.1 (only option)
        1. JPA implementation section
            1. Type: User Library
            1. "Disable Library Configuration" is also an option. I could try that.
            1. EclipseLink 4.0.2 is checked (I downloaded this previously with the "Download library..." button)
            1. "Include libraries with this application" is checked
        1. Connection section
            1. oracon2
                1. None is also an option
                1. When I choose oracon2, a little tooltip appears at the top that says: "Connection must be active to get data source specific help and validation."
            1. "Add driver library to build path" is unchecked
                1. I unchecked this because when I check it, I have to choose a driver from a dropdown menu, but the driver I have associated with the oracon2 connection does not appear in that drop-down menu, maybe becuase it is a "Generic JDBC driver".
    1. Click Next
    1. Web Module page
        1. Context root: TomeeDemo3
        1. Content directory: src/main/webapp
        1. "Generate web.xml deployment descriptor" is checked
    1. Click "Finish"
1. The project generates, and there is a problem in the problems view.
    1. web.xml: "Downloading external resources is disabled"
    1. The red underline is under the "xsi:schemaLocation" value.
        1. `"https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"`
    1. When I format the web.xml file, this error disappears.
    1. Closing the project, closing eclipse, starting eclipse, then opening the project also causes the error to disappear.
    1. Even though this error disappears so easily, I'm wondering if it's related to the error that prevents the project from starting, since they are both related to xml.
1. Right-click project, then click "Run As", then click "Run on server"

### Steps to fix

1. Google
   1. Query: java.lang.NoClassDefFoundError: org/eclipse/persistence/internal/jaxb/WrappedValue

   What is JAXB?
   What are Eclipse project facets?
