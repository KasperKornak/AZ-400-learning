# Integrating identity management systems
## GitHub SSO 
Offers both SAML and SCIM support. You need to connect your identity provider at the organization level to use GitHub SSO.

## Service Principals
Azure AD offers different kinds of mechanisms for authentication. In DevOps Projects, though, one of the most important is the use of **Service Principals**.

### Azure AD applications
Applications are registered with an Azure AD tenant within Azure Active Directory. Registering an application creates an identity configuration. You also determine who can use it:

- Accounts in the same organizational directory.
- Accounts in any organizational directory.
- Accounts in any organizational directory and Microsoft Accounts (personal).
- Microsoft Accounts (Personal accounts only).

Once the application is created, you then should create at least one client secret for the application.

The application identity can then be granted permissions within services and resources that trust Azure Active Directory.

To access resources, an entity must be represented by a security principal. To connect, the entity must know:

- TenantID.
- ApplicationID.
- Client Secret.

## Managed identity
There are two types of managed identities:

- **System-assigned**: It's the types of identities which are typical for most of Azure resources. Many, but not all, services expose these identities.
- **User-assigned**: You can create a managed identity as an Azure resource. It can then be assigned to one or more instances of a service.