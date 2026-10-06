PromisedLand Linux File Server
==============================

A Linux-based file server designed for a 38-user organization with departmental file sharing, centralized permissions, scalable storage, and layered security controls.

Overview
--------

This project involved designing and configuring a secure file server for a fictional organization, PromisedLand, expanding its infrastructure in Independence, Missouri.

The server was built using **Ubuntu Server 22.04 LTS** and provides centralized file storage for four departments:

*   Operations
    
*   Applications
    
*   CRM
    
*   Finance
    

The environment uses **Samba, Linux groups, POSIX permissions, ACLs, LVM, UFW, Fail2Ban, and SSH hardening**to provide controlled and secure access to shared resources.

Project Goals
-------------

*   Provide centralized departmental file storage
    
*   Restrict users to the resources appropriate for their department
    
*   Provide read-only access for an executive user across departments
    
*   Use LVM to support future storage expansion
    
*   Provide Windows-compatible network file sharing through Samba
    
*   Implement multiple layers of system security
    
*   Automate user and group creation for consistency
    

Architecture
------------

The server uses two virtual disks:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   Disk 1  └── Ubuntu Server OS      └── / (ext4)  Disk 2  └── LVM      └── Volume Group: vg_data          ├── lv_operations          ├── lv_applications          ├── lv_crm          └── lv_finance   `

Shared directories are mounted under /srv:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   /srv/  ├── operations/  ├── applications/  ├── crm/  └── finance/   `

Technologies Used
-----------------

TechnologyPurposeUbuntu Server 22.04 LTSServer operating systemSambaSMB/CIFS network file sharingLVMFlexible storage managementLinux GroupsDepartment-based access controlPOSIX ACLsGranular permissionsUFWHost-based firewallFail2BanBrute-force protectionOpenSSHSecure remote administrationNetplanStatic network configurationBashUser and group automation

Users and Groups
----------------

A total of **38 users** were created based on the organization's organizational structure.

Department groups:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   operations  applications  crm  finance  exec_read   `

Users were created through a shell script rather than manually, improving consistency and reducing repetitive administrative work.

Each department's users are restricted to their corresponding shared directory.

An executive account, cshumaker, was configured with read-only access across the shared department directories using ACLs.

Storage Design
--------------

The server separates the operating system from application data.

The operating system resides on the first virtual disk, while the second disk is dedicated to departmental storage through LVM.

Logical volumes:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   vg_data  ├── lv_operations  ├── lv_applications  ├── lv_crm  └── lv_finance   `

This design allows individual departmental volumes to be managed and expanded independently as storage requirements grow.

File Permissions
----------------

Department directories are protected using Linux groups and ACLs.

For example, the executive user was granted read and execute access to departmental directories:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo setfacl -m u:cshumaker:rx /srv/operations  sudo setfacl -m u:cshumaker:rx /srv/applications  sudo setfacl -m u:cshumaker:rx /srv/crm  sudo setfacl -m u:cshumaker:rx /srv/finance   `

Samba was then configured to provide authenticated network access to the shared directories.

Security
--------

Several security controls were implemented.

### UFW

The firewall was configured to restrict inbound traffic to required services, including SSH and Samba.

### Fail2Ban

Fail2Ban was configured to help protect the server against repeated authentication attempts and brute-force attacks.

### SSH Hardening

Remote administration was secured by disabling direct root login and restricting authentication.

### Static Networking

The server was configured with a static IP address:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   192.168.100.99   `

using Netplan.

### System Updates

The server was maintained using Ubuntu's package management tools:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo apt update  sudo apt upgrade   `

Automation
----------

A Bash script was created to automate user creation based on the organizational structure.

This reduced manual configuration and helped ensure that users were consistently assigned to the correct groups.

What I Learned
--------------

This project strengthened my practical experience with:

*   Linux server administration
    
*   Samba configuration
    
*   Linux permissions and ACLs
    
*   LVM storage management
    
*   User and group administration
    
*   Bash scripting
    
*   SSH security
    
*   Firewall configuration
    
*   Fail2Ban
    
*   Network configuration
    
*   Designing infrastructure around organizational requirements
    

Project Demonstration
---------------------

A video walkthrough of the project is available here:

[Watch the Project Demonstration](https://youtu.be/wQaYPyuvUEA)

Project Documentation
---------------------

The original project documentation and architecture diagrams can be included in the repository under:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   docs/  ├── architecture/  ├── storage/  └── screenshots/   `

Key Takeaway
------------

This project demonstrates how I approached a realistic systems administration scenario by translating organizational requirements into a **secure, structured, and scalable Linux file-server environment**.

Project Demonstration
---------------------

[Watch the full project demonstration on YouTube](https://youtu.be/wQaYPyuvUEA)