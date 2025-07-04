Apache filesystem:
This is the default documentation page, which may not be removed by administrators. Here is the general folder structure of a Tomcat installation.

  Tomcat - Discovery & Enumeration
├── bin
├── conf
│   ├── catalina.policy
│   ├── catalina.properties
│   ├── context.xml
│   ├── tomcat-users.xml
│   ├── tomcat-users.xsd
│   └── web.xml
├── lib
├── logs
├── temp
├── webapps
│   ├── manager
│   │   ├── images
│   │   ├── META-INF
│   │   └── WEB-INF
|   |       └── web.xml
│   └── ROOT
│       └── WEB-INF
└── work
    └── Catalina
        └── localhost

The bin folder stores scripts and binaries needed to start and run a Tomcat server. The conf folder stores various configuration files used by Tomcat. The tomcat-users.xml file stores user credentials and their assigned roles. The lib folder holds the various JAR files needed for the correct functioning of Tomcat. The logs and temp folders store temporary log files. The webapps folder is the default webroot of Tomcat and hosts all the applications. The work folder acts as a cache and is used to store data during runtime.

Each folder inside webapps is expected to have the following structure.

  Tomcat - Discovery & Enumeration
webapps/customapp
├── images
├── index.jsp
├── META-INF
│   └── context.xml
├── status.xsd
└── WEB-INF
    ├── jsp
    |   └── admin.jsp
    └── web.xml
    └── lib
    |    └── jdbc_drivers.jar
    └── classes
        └── AdminServlet.class   
The most important file among these is WEB-INF/web.xml, which is known as the deployment descriptor. This file stores information about the routes used by the application and the classes handling these routes. All compiled classes used by the application should be stored in the WEB-INF/classes folder. These classes might contain important business logic as well as sensitive information. Any vulnerability in these files can lead to total compromise of the website. The lib folder stores the libraries needed by that particular application. The jsp folder stores Jakarta Server Pages (JSP), formerly known as JavaServer Pages, which can be compared to PHP files on an Apache server.
