# Debug Journal

## Entry 3

### System details

OS: Windows 11 Professional

Eclipse: "Eclipse IDE for Enterprise Java and Web Developers" Version: 2024-12 (4.34.0)

Java: Adoptium 21

TomEE: 10.0

Oracle DB: 21c XE

### Project

[TomeeDemo4](https://github.com/wharvex/TomeeDemo4)

### Retry of

N/A

### Goal

Create a servlet that posts "hello world" to `http://localhost:<port>/TomeeDemo4/` when I run the project.

### Goal achieved?

??

### Main error

??

### Expected behavior

When I run the project, a browser window opens, navigating to `http://localhost:<port>/TomeeDemo4/`, which displays "Hello world"

### Reason for expecting this behavior

This is the behavior I observed when I ran a Websphere Java app with a servlet.

### Actual behavior

??

### Steps to reproduce

1. Window -- Powershell
    1. Issue command: `C:\eclipse-jee\eclipse\eclipse.exe -consoleLog`
        1. (Eclipse IDE window opens)
        1. (Eclipse Console window opens for log output; if you close this window, the IDE window will also close)
1. Window -- Eclipse IDE
    1. View -- Package Explorer
        1. Right-click: TomeeDemo4 (project)
        1. Context menu
            1. Click: Open Project (if not already open)
            1. Click: New `->` Other
    1. Window -- Select a Wizard
        1. Box
            1. Expand: Java
            1. Click: Package
        1. Click: Next
            1. (The window becomes the "New Java Package" creation wizard)
    1. Window -- New Java Package
        1. Textbox -- Source folder
            1. Enter: TomeeDemo4/src/main/java
        1. Textbox -- Name
            1. Enter: com.wharvex.demo.tomee4
        1. Checkbox -- Create package-info.java
            1. Do: Uncheck
        1. Click: Finish `->` 
            1. (Package generates and is visible in the package explorer, but Git detects no changes, i.e. it hasn't created the folder yet)
    1. View -- Package Explorer
        1. Expand: TomeeDemo4
        1. Expand: src/main/java
        1. Right-click: com.wharvex.demo.tomee4
        1. Context menu
            1. New `->` Servlet
        1. Window -- Create Servlet
            1. Dropdown -- Project
                1. Click: TomeeDemo4 (only choice)
            1. Textbox -- Source folder
                1. Enter: /TomeeDemo4/src/main/java
            1. Textbox -- Java package
                1. Enter: com.wharvex.demo.tomee4
            1. Textbox -- Class name
                1. Enter: HelloWorldServlet
            1. Textbox -- Superclass
                1. Enter: jakarta.servlet.http.HttpServlet
            1. Checkbox -- Use an existing Servlet class or JSP
                1. Do: Uncheck
        1. Click: Finish
1. Right-click project, then click "Run As", then click "Run on server"

### Thoughts

1. OpenJPA does appear in the console output.
    1. INFO: You have enabled runtime enhancement, but have not specified the set of persistent classes.  OpenJPA must look for metadata for every loaded class, which might increase class load times significantly.
    1. INFO: OpenJPA dynamically loaded a validation provider.
1. Does OpenJPA appear in the console output for entry 1?
    1. No! That's encouraging. It seems to suggest that TomEE "fell back" to OpenJPA when I didn't choose a user library for the JPA implementation, which is what I was hoping would happen.

### Next Steps

1. Make a Servlet hello world
1. Make a JDBC hello world
1. Make a JPA hello world
