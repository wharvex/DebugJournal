# Debug journal

## Entry 1

### Goal

Run a newly-created (i.e. homepage-less) TomEE Dynamic Web Project (DWP) **_with a JPA facet_** in Eclipse.

The project name is TomeeDemo3.

### Expected behavior

A browser window opens, navigating to http://localhost:8080/TomeeDemo3/, which displays a "404" page supplied by TomEE.

### Reason for expecting this behavior

This is the behavior I observed when I ran a newly-created TomEE DWP **_without a JPA facet_** in Eclipse.

### Actual behavior

* Eclipse displays a small "Problem Occurred" popup window with no useful information.
* A browser window opens, navigating to http://localhost:8080/TomeeDemo3/, which displays a "local host connection refused" page supplied by Microsoft Edge.

![eclipse problem occurred popup](https://github.com/wharvex/DebugJournal/blob/main/Entry1/eclipse_problem-occurred.png)

![edge localhost connection refusal page](https://github.com/wharvex/DebugJournal/blob/main/Entry1/edge_refusal.png)

### Error received

java.lang.NoClassDefFoundError: org/eclipse/persistence/internal/jaxb/WrappedValue

### Steps to reproduce

1. Create a DWP in Eclipse
   1. Runtime: TomEE
   1. JPA facet
      1. Database: Oracle DB
      1. Library (implementation): EclipseLink
         1. I would rather use OpenJPA which supposedly is bundled with TomEE, but Eclipse does not (explicitly) give me that option
            1. Maybe if I don't choose a library, it will default to OpenJPA, I'll try that next
1. Right-click project, then click "Run As", then click "Run on server"

### Steps to fix

1. Google
   1. Query: java.lang.NoClassDefFoundError: org/eclipse/persistence/internal/jaxb/WrappedValue
