---
layout: post
title:  Debugging-Tomcat-Remotely-Using-Eclipse
date:   2018-07-24 09:20:00 +0900
categories: MD
comments : true
---

# Debugging Tomcat as a Remote External Application

 > Eclipse can be configured to provide debugging information for a running tomcat instance that is configured with JPDA support.  This approach may work better for users using Windows.  

Instructions for setting up tomcat to use JPDA.

Once you have tomcat configured:

1. Start tomcat manually from the command line.
2. Set a breakpoint somewhere in your code (preferably something not associated with startup) by left-clicking to the left of a line of code.
3. Under the "Run" menu, select "Debug"
4. Single-click the heading "Remote Java Application", then press the new button (looks like a page with a plus on it).
5. On the dialog that appears, enter a meaningful name (like "Sakai (remote)" for this configuration).
6. From the same dialog, change to the "Source" tab, then click "Add".
7. On the dialog that appears, single-click "Java Project", then click "OK".
8. On the dialog that appears, click "Select All" (or check the projects you wish to import), then click "OK".
9. Now click the "Connect" tab and check the box marked "Allow termination of remote JVM".
10. Click "Apply" to save your changes.
11. Click "Debug" to start testing your configuration.
12. Open your browser and work with Sakai until you reach your breakpoint.  You should be directed into Eclipse where you will see the line of code with your breakpoint, a list of variables, etc., etc.

```If you do not see the console output when using this method, you can simply keep open a separate window that displays the last contents of catalina.out.```

# Debugging Tomcat as an External Tool from within Eclipse

 > You can also configure Eclipse to be able to start and stop tomcat as a program (this approach also seems to work well on Windows).  To configure Eclipse to be able to start and stop tomcat:

```If you have already completed the above steps, you will need to stop tomcat to use the steps below.```

1. If you have not already done so, set a breakpoint somewhere in your code (preferably something not associated with startup) by left-clicking to the left of a line of code.
2. Under the "Run" menu, select the "External Tools" sub-menu, then the "External Tools" heading.
3. On the dialog that appears, single-click the "Program" heading, then click the "New" icon (a page with a plus on it).
4. Change the Name to something meaningful, like "Sakai 2.4".
5. Under "Location", enter full path and filename for your catalina script (catalina.sh on UNIX, catalina.bat on Windows).
6. Under "Working Directory" to the location of your tomcat installation.
7. Under "Arguments", enter "jpda start".	 NOTE: If you use "jdpa run" rather than "jpda start", Tomcat will not open a new window to start in and will instead give you all it's standard err and standard out in the Eclipse console window. This is usefull for Windows developers who don't like to read console output from a DOS window.
8. Go to the "Environment" tab.
9. Click "New" and enter "JPDA_ADDRESS" as the name and "8000" as the value, then hit "OK".
10. Click "New" again and enter "JPDA_TRANSPORT" as the name and "dt_socket" as the value, then hit "OK".
11. Click "New" again and enter "JAVA_OPTS" as the name and "-server -XX:+UseParallelGC -Xmx768m -XX:MaxPermSize=160m -Djava.awt.headless=true" as the value, then hit "OK".
12. Open the "Common" tab.
13. Check the box marked "External Tools" in the panel marked "Display in favorites menu".
14. Check the box marked "Launch in background".
15. Hit "Apply" to save your changes.
16. Hit "Run" to test your configuration.
17. Once Sakai finishes starting up, open your browser and work with Sakai until you reach your breakpoint. You should be directed into Eclipse where you will see the line of code with your breakpoint, a list of variables, etc., etc.

Once you exit the debugger and stop tomcat, you should be able to start and stop tomcat with debugging enabled from within Eclipse by opening the "Run" menu and selecting "External Tools".  You can also begin debugging your code by opening the "Run" menu and selecting "Debug".

```tomcat will start when you run it as an external tool, but you will probably not see console output.  You'll need to open another window that contains the contents of catalina.out to keep track of the startup or use jpda run```

# Configuring Tomcat as a Known Server from within Eclipse

 > You can also configure Eclipse to be able to start and stop tomcat as a server (this approach seems to work well on Unix).  To configure Eclipse to be aware of tomcat as a server:

```If you have already completed the above steps, you will need to stop tomcat to use the steps below.```

1. Edit ~/eclipse/eclipse.ini and change the memory settings as follows: 

```
-vmargs
-Xms256m
-Xmx1024m
```

 * <b>NOTE:</b> Eclipse has to have enough available memory to actually run Sakai within it

2. If you have not already done so, set a breakpoint somewhere in your code (preferably something not associated with startup) by left-clicking to the left of a line of code.

3. Configure Eclipse to launch the desired JVM with the appropriate memory settings:	 
 a. Under the "Eclipse" menu, select "Preferences"
 b. Open the "Java" heading and the "Installed JREs" subheading.
 c. Single-click the desired run-time, then click "Edit".
 d. Under "Default VM Arguments", enter "-server -XX:+UseParallelGC -Xmx1024m -XX:MaxPermSize=384m -Djava.awt.headless=true", then click "OK".
 e. Click "OK" again to close the preferences dialog.

4. Add the Sakai tomcat instance to the list of known runtimes:	 
 -. Under the "Eclipse" menu, select "Preferences"
 -. Expand the "Server" heading on the left side of the dialog that appears, then single-click "Installed Runtimes" and click "Add".
 -. On the dialog that appears, expand the Apache heading, then single-click "Apache Tomcat v5.5" and click "Next".\
 -. Under "Name", enter something meaningful like "Sakai 2.4 tomcat".
 -. Under "Tomcat installation directory", enter the location of your tomcat home directory.
 -. Under "JRE", select the JVM you configured above.
 -. Click "Finish".

5. Add the server to the debug view	 Open the "Debug" view.
 a. Open the "Servers" tab.
 b. Using the right mouse button (or control-click on the mac), open the dialog to create a new server.
 c. Under "Server's host name", fill in "localhost" if it doesn't already appear.
 d. Open the "Apache" heading, then select "Tomcat v5.5 Server".
 e. Under "Server Runtime", select the runtime you defined earlier.
 f. Click "Finish" (you're not actually finished  )
 g. Double-click the new server entry, a dialog should appear with more advanced options.
 h. Change "Server name" to something meaningful like "Sakai 2.4 (Server)".
 i. Uncheck "Run modules directly from the workspace".
 j. Click "Open launch configuration".
 k. On the dialog that appears, open the "Arguments" tab.  Change "program arguments" to "jpda start".		 
 
  * <b>NOTE:</b> If you use "jdpa run" rather than "jpda start", Tomcat will not open a new window to start in and will instead give you all it's standard err and standard out in the Eclipse console window. This is usefull for Windows developers who don't like to read console output from a DOS window.

 l. On the same dialog, change "VM arguments" so that catalina.base is set to the tomcat home directory.
 m. Open the "Source" tab, then click "Add".
 n. On the dialog that appears, single-click "Java Project", then click "OK".
 o. On the dialog that appears, click "Select All", then click "OK".
 p. Open the "Environment" tab, then click "New".
 r. Enter "JPDA_ADDRESS" for "Name", and "8000" for "Value", then click "OK".
 s. Click "Add" again, then enter "JPDA_TRANSPORT" for "Name" and "dt_socket" for "Value".  Click "OK".
 t. Click "Apply", then "OK" to save your launch settings.
 u. <b>IMPORTANT:</b>  Under the "File" menu, select "Save" to save your overall server definition.
 v. You should now be able to start up the server by right clicking (or control clicking) its heading in the "Servers" tab and selecting "Start".
 w. Once Sakai finishes starting up, open your browser and work with Sakai until you reach your breakpoint. You should be directed into Eclipse where you will see the line of code with your breakpoint, a list of variables, etc., etc.
 
