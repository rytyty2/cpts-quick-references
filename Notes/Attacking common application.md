# Attacking Servlet Containers/Software Development

### Tomcat - Discovery & Enumeration

Discover version from error page:

<img width="918" height="220" alt="image" src="https://github.com/user-attachments/assets/dba5c286-dbd2-41cb-9dfd-f63cecd0e697" />

Method: insert invalid page 


The _bin_ folder stores scripts and binaries needed to start and run a Tomcat server. 

The _conf_ folder stores various configuration files used by Tomcat. 

The **_tomcat-users.xml_** file stores user credentials and their assigned roles. 

The _lib_ folder holds the various JAR files needed for the correct functioning of Tomcat. 

The _logs_ and temp folders store temporary log files. 

The _webapps_ folder is the default webroot of Tomcat and hosts all the applications. 

The _work_ folder acts as a cache and is used to store data during runtime.


Default tree:

<img width="924" height="508" alt="image" src="https://github.com/user-attachments/assets/00ac2510-9533-4eb1-af5e-ba28d7f46843" />

<img width="940" height="377" alt="image" src="https://github.com/user-attachments/assets/e5f689c2-a319-4b3a-84e0-dec24c9b99a1" />

The most important file among these is WEB-INF/web.xml, which is known as the deployment descriptor. This file stores information about the routes used by the application and the classes handling these routes. All compiled classes used by the application should be stored in the WEB-INF/classes folder. These classes might contain important business logic as well as sensitive information. Any vulnerability in these files can lead to total compromise of the website. The lib folder stores the libraries needed by that particular application. The jsp folder stores Jakarta Server Pages (JSP), formerly known as JavaServer Pages, which can be compared to PHP files on an Apache server.

### Tomcat attack path:

War upload 
https://en.wikipedia.org/wiki/WAR_(file_format)#:~:text=In%20software%20engineering%2C%20a%20WAR,that%20together%20constitute%20a%20web

https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp

Direct vulnerabilities like: CVE-2020-1938 : Ghostcat
