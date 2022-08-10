# Active Directory

# Table of contents

* [Active Directory](README.md)
  * [1. Domain Enumeration](active-directory/1.-domain-enumeration/README.md)
    * [Domain enumeration](<1. Domain Enumeration/01. DomainEnum.md>)
    * [User Enumeration](<1. Domain Enumeration/02. UserEnum.md>)
    * [Computer Enumeration](<1. Domain Enumeration/03. ComputerEnum.md>)
    * [Group Enumeration](<1. Domain Enumeration/04. GroupEnum.md>)
    * [Logged Users](<1. Domain Enumeration/05. LoggedUsers.md>)
    * [Shares, sensitive files and file-servers enumeration](<1. Domain Enumeration/06. FileShareEnum.md>)
    * [GPO (Group Policy Object) Enumeration](<1. Domain Enumeration/07. GPOEnum.md>)
    * [OU Enumeration](<1. Domain Enumeration/08. OUEnum.md>)
    * [ACL Enumeration](<1. Domain Enumeration/09. ACLEnum.md>)
    * [Trusts enumeration](<1. Domain Enumeration/10. TrustEnum.md>)
    * [User Hunting](<1. Domain Enumeration/11. UserHunting.md>)
    * [BloodHound](<1. Domain Enumeration/12. BloodHound.md>)
  * [2. Local Privesc](active-directory/2.-local-privesc/README.md)
    * [Local Privilege Escalation](<2. Local Privesc/01. LocalPrivesc.md>)
  * [3. Pivoting Techniques](active-directory/3.-pivoting-techniques/README.md)
    * [Pivoting when we have local admin privs on other machines](<3. Pivoting Techniques/1. LocalPrivsOnOtherMachines.md>)
    * [PowerShell Reverse shell](<3. Pivoting Techniques/2. PowershellReverShell.md>)
    * [Powershell Remoting](<3. Pivoting Techniques/3. Powershell Remoting.md>)
    * [Pass The Hash](<3. Pivoting Techniques/4. PassTheHash.md>)
    * [Over Pass The Hash](<3. Pivoting Techniques/5. OverPassTheHash.md>)
    * [Constrained Language / AppLocker](<3. Pivoting Techniques/6. ConstrainedLanguage-AppLocker.md>)
  * [4. Evasion Techniques](active-directory/4.-evasion-techniques/README.md)
    * [AMSI Bypass](<4. Evasion Techniques/1. AMSI bypass.md>)
    * [Disable Windows Defender](<4. Evasion Techniques/2. Disable Windows Defender.md>)
    * [Exclusion Paths](<4. Evasion Techniques/3. Exclusion Paths.md>)
  * [5. Persistance](active-directory/5.-persistance/README.md)
    * [Kerberos](<5. Persistance/0. Kerberos - How it works.md>)
    * [Mimikatz](<5. Persistance/1. Mimikatz.md>)
    * [Golden Ticket](<5. Persistance/2. Golden Ticket.md>)
    * [Silver Ticket](<5. Persistance/3. Silver Ticket.md>)
    * [Skeleton Key](<5. Persistance/4. Skeleton Key.md>)
    * [DSRM (Directory Service Restore Mode)](<5. Persistance/5. DSRM.md>)
    * [Custom SSP (Security Support Provider)](<5. Persistance/6. Custom SSP.md>)
    * [AdminSDHolder](<5. Persistance/7. AdminSDHolder.md>)
    * [Domain Object](<5. Persistance/8. DomainObject.md>)
    * [Security Descriptors (Remoting methods)](<5. Persistance/9. Security Descriptors.md>)
  * [6. Domain PrivEsc](active-directory/6.-domain-privesc/README.md)
    * [Kerberoast](<6. Domain PrivEsc/1. Kerberoast.md>)
    * [Kerberoast AS-REPs](<6. Domain PrivEsc/2. Kerberoast -AS-REPs.md>)
    * [Kerberoast - Set SPN](<6. Domain PrivEsc/3. Kerberoast - Set SPN.md>)
    * [Unconstrained Delegation](<6. Domain PrivEsc/4. Unconstrained Delegation.md>)
    * [Constrained Delegation](<6. Domain PrivEsc/5. Constrained Delegation.md>)
    * [DNSAdmins](<6. Domain PrivEsc/6. DNSAdmins.md>)
    * [Cross Domain Attacks](<6. Domain PrivEsc/7. Cross Domain Attacks.md>)
    * [Cross Forest Attack](<6. Domain PrivEsc/8. Cross Forest Attack.md>)
    * [Database Links](<6. Domain PrivEsc/9. Database Links.md>)
  * [7. Forest Persistance](active-directory/7.-forest-persistance/README.md)
    * [Forest Persistance - DCShadow](<7. Forest Persistance/1. DCShadow.md>)
