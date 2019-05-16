# Goal
Detect the existence of a web shell (web script) on an openly accessible Windows web server.

# Categorization
These techniques are categorized as [Persistence / Webshell](https://attack.mitre.org/techniques/T1100/).

# Strategy Abstract
The strategy will function as follows: 

* Develop a regular change management policy for asset owner's web servers. 
* Compare changes to servable content using a file integrity system via endpoint tooling.
* Alert on any discrepancies between desired and current states.

# Technical Context
A web shell is a server-side compromise allowing an adversary to establish a persistent connection to a computer network.  A web shell can be written in any web-based scripting language, with the most common being .asp, .aspx, .js, .jsp, .pl, .rb, .py, .sh, or .php.  They can be written as complex stand-alone files, or a simple line of code injected into an existing web page. 

Using network reconnaissance tools, an adversary can identify vulnerabilities that can be exploited and result in the installation of a web shell.  For example, these vulnerabilities can exist in content management systems (CMS) or web server software.  Often web shells are placed on the server itself by exploiting vulnerabilities or misconfigurations, but web shells can also be placed on the server via lateral movement from other compromised accounts or hosts.  Once successfully uploaded, an adversary can use the web shell to leverage other exploitation techniques to escalate privileges and to issue commands remotely.  These commands are directly linked to the privilege and functionality available to the web server and may include the ability to add, delete, and execute files; as well as the ability to run shell commands, additional executables, or scripts.

# Blind Spots and Assumptions
This strategy relies on the following assumptions: 
* Windows event logs are being successfully generated on Windows web servers.
* Windows event logs are successfully forwarded to WEF servers. 
* Endpoint tooling (file integrity system such as Tripwire or Samhain) is running and functioning correctly on the system's specified web directories.
* Any web directory designated as a repository for user-generated content should enforce file permissions such that content isn't allowed to be executable.

A blind spot will occur if any of the assumptions are violated. For instance, the following would not trip the alert: 
* Windows event forwarding or auditing is disabled on the host.
* Endpoint tooling is disabled or modified. 

# False Positives
There are very limited instances where false positives will occur: 
* Web developers push a change list outside the purview of the file integrity system.
* File integrity system crashes or is disabled during a change list push.

# Priority
The priority is set to high under the following conditions:
* File system integrity tooling reports an unauthorized change in file system's web directories.

# Validation
Validation can occur for this ADS by creating or modifying a file within the web server directory structure.
This validation scenario should trigger a detection event by the file integrity monitoring system.

# Response
In the event that this alert fires, the following response procedures are recommended:
* Identify the file(s) that were modified.
* Validate the user/group responsible for the change
  * If not a true positive, verify file integrity monitoring system is running.
  * If it is a true positive, escalate to incident response.
* If file integrity checks were bypassed, treat as a high priority alert. 
  * Investigate and record any processes which have executed since the last reboot.
  * If the system owner is unaware of this behavior, escalate to incident response.

# Additional Resources
* [Compromised Web Servers and Web Shells - Threat Awareness and Guidance](https://www.us-cert.gov/ncas/alerts/TA15-314A)
* [Detecting Web Shells in HTTP access logs](https://www.anomali.com/blog/detecting-web-shells-in-http-access-logs)
