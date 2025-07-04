Apache filesystem:
This is the default documentation page, which may not be removed by administrators. Here is the general folder structure of a Tomcat installation.

![image](https://github.com/user-attachments/assets/afbcc57e-1cc4-471c-95c1-dcc5c2ca208d)


The bin folder stores scripts and binaries needed to start and run a Tomcat server. The conf folder stores various configuration files used by Tomcat. The tomcat-users.xml file stores user credentials and their assigned roles. The lib folder holds the various JAR files needed for the correct functioning of Tomcat. The logs and temp folders store temporary log files. The webapps folder is the default webroot of Tomcat and hosts all the applications. The work folder acts as a cache and is used to store data during runtime.

Each folder inside webapps is expected to have the following structure.

  Tomcat - Discovery & Enumeration

![image](https://github.com/user-attachments/assets/25d73346-9bb9-4ae7-9a37-5eaf04d1ca06)

 
The most important file among these is WEB-INF/web.xml, which is known as the deployment descriptor. This file stores information about the routes used by the application and the classes handling these routes. All compiled classes used by the application should be stored in the WEB-INF/classes folder. These classes might contain important business logic as well as sensitive information. Any vulnerability in these files can lead to total compromise of the website. The lib folder stores the libraries needed by that particular application. The jsp folder stores Jakarta Server Pages (JSP), formerly known as JavaServer Pages, which can be compared to PHP files on an Apache server.
