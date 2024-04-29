![[Pasted image 20240308223722.png]]

Robert R. King:
>A domain represents a database. That database holds records about network services - things like computers, users, groups and other things that use, support, or exist on a network. The domain database is, in effect, Active Directory.

## Active Directory Services

Source: https://en.wikipedia.org/wiki/Active_Directory

Active Directory Services consist of multiple directory services. The best known is Active Directory Domain Services, commonly abbreviated as AD DS or simply AD.
### Domain Services
Active Directory Domain Services (AD DS) is the foundation of every Windows domain network. It stores information about domain members, including devices and users, verifies their credentials, and defines their access rights. The server running this service is called a domain controller. A domain controller is contacted when a user logs into a device, accesses another device across the network, or runs a line-of-business Metro-style app sideloaded into a machine.

Other Active Directory services (excluding LDS, as described below) and most Microsoft server technologies rely on or use Domain Services; examples include Group Policy, Encrypting File System, BitLocker, Domain Name Services, Remote Desktop Services, Exchange Server, and SharePoint Server.

### Lightweight Directory Services

Active Directory Lightweight Directory Services (AD LDS), previously called Active Directory Application Mode (ADAM), implements the LDAP protocol for AD DS. It runs as a service on Windows Server and offers the same functionality as AD DS, including an equal API. However, AD LDS does not require the creation of domains or domain controllers. It provides a Data Store for storing directory data and a Directory Service with an LDAP Directory Service Interface. Unlike AD DS, multiple AD LDS instances can operate on the same server.

### Certificate Services

Active Directory Certificate Services (AD CS) establishes an on-premises public key infrastructure. It can create, validate, revoke and perform other similar actions, public key certificates for internal uses of an organization. These certificates can be used to encrypt files (when used with Encrypting File System), emails (per S/MIME standard), and network traffic (when used by virtual private networks, Transport Layer Security protocol or IPSec protocol).

AD CS predates Windows Server 2008, but its name was simply Certificate Services.

AD CS requires an AD DS infrastructure.

### Federation Services

Active Directory Federation Services (AD FS) is a single sign-on service. With an AD FS infrastructure in place, users may use several web-based services (e.g. internet forum, blog, online shopping, webmail) or network resources using only one set of credentials stored at a central location, as opposed to having to be granted a dedicated set of credentials for each service. AD FS uses many popular open standards to pass token credentials such as SAML, OAuth or OpenID Connect. AD FS supports encryption and signing of SAML assertions. AD FS's purpose is an extension of that of AD DS: The latter enables users to authenticate with and use the devices that are part of the same network, using one set of credentials. The former enables them to use the same set of credentials in a different network.

As the name suggests, AD FS works based on the concept of federated identity.

AD FS requires an AD DS infrastructure, although its federation partner may not.

### Rights Management Services

Active Directory Rights Management Services (AD RMS), previously known as Rights Management Services or RMS before Windows Server 2008, is server software that allows for information rights management, included with Windows Server. It uses encryption and selective denial to restrict access to various documents, such as corporate e-mails, Microsoft Word documents, and web pages. It also limits the operations authorized users can perform on them, such as viewing, editing, copying, saving, or printing. IT administrators can create pre-set templates for end users for convenience, but end users can still define who can access the content and what actions they can take.