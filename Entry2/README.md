# Debug Journal

## Entry 2

### System details

OS: Windows 11 Professional

Eclipse: "Eclipse IDE for Enterprise Java and Web Developers" Version: 2024-12 (4.34.0)

Java: Adoptium 21

TomEE: 10.0

Oracle DB: 21c XE

### Project name

TomeeDemo4

### Goal

Run a newly-created TomEE Dynamic Web Project (DWP) **_with a JPA facet_** in Eclipse.

It is newly-created in the sense that it does not have any static homepage, it does not have a servlet, it does not even have a main Java package. I completed the DWP creation wizard and then immediately tried to run it.

This time, as opposed to in Entry 1, I will not choose a User Library for the JPA implementation.

### Goal achieved?

Yes.

### Main error

N/A

### Expected behavior

A browser window opens, navigating to http://localhost:xxxx/TomeeDemo3/, which displays a "404" page supplied by TomEE.

### Reason for expecting this behavior

This is the behavior I observed when I ran a newly-created TomEE DWP **_without a JPA facet_** in Eclipse.

### Actual behavior

A browser window opens, navigating to http://localhost:xxxx/TomeeDemo3/, which displays a "404" page supplied by TomEE.

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
                1. Select: Disable Library Configuration
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
            1. Enter: TomeeDemo4
        1. Textbox -- Content directory
            1. Enter: src/main/webapp
        1. Checkbox -- Generate web.xml deployment descriptor
            1. Do: Check
        1. Click: Finish
1. The project generates
    1. If there is a problem in the problems view "Downloading external resources is disabled", go to Window `->` Preferences `->` XML (Wild Web Developer) `->` Check "Download external resources like referenced DTD, XSD"
1. Right-click project, then click "Run As", then click "Run on server"
