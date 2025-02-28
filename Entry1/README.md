# Debug journal

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

It is newly-created in the sense that it does not have any static homepage, it does not have a servlet, it does not even have a main Java package. I completed the DWP creation wizard, closed the project, closed Eclipse, opened Eclipse, opened the project, and then tried to run it.

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

1. Open 
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
                1. Panel -- Facets
                    1. Check: Dynamic Web Module; Select: 6.0
                    1. Check: Java; Select: 21
                    1. Check: JavaScript; Select: 1.0
                    1. Check: JPA; Select: 3.1
                    1. _NEXT STEPS IDEA: JAXB facet is also available here, I did not check it, I could try that_
                1. Panel -- Runtimes
                    1. Check: TomEE (only option)
                1. Click: OK `->` "Project Facets" window closes, Configuration dropdown now has `<custom>` selected
        1. Section -- EAR membership
            1. Inaccessible
        1. Section -- Working sets
            1. Checkbox -- Add project to working sets
                1. Do: Uncheck
        1. Click: Next
    1. Page -- Java
        1. Section -- Source folders on build path
            1. Ensure exactly 1 entry for: src\main\java
        1. Textbox -- Default output folder
            1. Enter: src\main\webapp\WEB-INF\classes
        1. Click: Next
    1. Page -- JPA Facet
        1. Section: Platform
            1. Dropdown
                1. Generic 3.1 (only option)
        1. Section: JPA implementation
            1. Dropdown: Type
                1. User Library
                1. _NEXT STEPS IDEA: "Disable Library Configuration" is also an option, I could try that_
            1. "EclipseLink 4.0.2" is checked (I downloaded this previously with the "Download library..." button)
            1. Checkbox: Include libraries with this application
                1. Checked
        1. Section: Connection
            1. Dropdown
                1. oracon2 (I made this connection profile previously)
                1. _NEXT STEPS IDEA: None is also an option. I could try that._
            1. Checkbox: Add driver library to build path
                1. Unchecked
                    1. I unchecked this because when I check it, I have to choose a driver from a dropdown menu, but the driver I have associated with the oracon2 connection does not appear in that drop-down menu, maybe becuase it is a "Generic JDBC driver".
        1. Section: Persistent class management
            1. Radio button group
                1. Annotated classes must be listed in persistence.xml
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
