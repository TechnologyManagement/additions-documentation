Extensions
==========

We provide our Additions functionality where possible as a Dynamics NAV
extension (.navx) file. But [why](#why-extensions) not just provide objects?
[How do extensions work](#how-do-extensions-work)?

Why Extensions?
---------------

Extensions are a way of packaging NAV functionality that was introduced in NAV
2016. The major benefit is that they allow some functionality to be applied to a
target database without impacting any existing development.

### No Merging

With traditional object deployment, any changes to standard NAV objects must be
merged – either using PowerShell cmdlets or working with text files of objects
manually.

With extensions, you take the .navx file that we supply, use PowerShell to
publish and install the extension into the database and leave NAV to worry about
merging the functionality of each object.

### Uninstalling

When some functionality is installed as an extension, removing that
functionality is as easy as installing it. Removing code and table fields in
your customer’s database is not without risk. If you need to remove the
extension just run the Uninstall-NavApp cmdlet – the data will be safely
archived and the functionality removed from the database.

How do Extensions Work?
-----------------------

Overview
--------

There is thorough documentation on how extensions work and how to deploy them on
MSDN: <https://msdn.microsoft.com/en-us/library/mt574417%28v=nav.90%29.aspx>

The process to apply an extension to a database is simple:

1.  Copy the navx file to the NAV service tier machine

2.  Open a PowerShell ISE session as administrator

3.  Import the NAV PowerShell modules

4.  Publish the extension to a service tier connected to the target database

5.  Install the extension to the target service tier/tenant

6.  Synchronise the target tenant(s)

7.  Restart any client sessions that were logged in prior to the installation to
    see menu changes

PowerShell
----------

Open PowerShell ISE as administrator to start a new session. This session will
allow us to import NAV modules and execute commands against the service tier to
publish and install the extensions.

### Modules

NAV includes several PowerShell modules which allow you to work with the service
tier, objects and extensions. Import these modules with the *Import-Module*
command in PowerShell ISE as follows. Note: these are the default installation
paths for NAV 2017, change as appropriate to your installation.

Import-Module "C:\\Program Files\\Microsoft Dynamics
NAV\\100\\Service\\Microsoft.Dynamics.Nav.Apps.Management.psd1"  
Import-Module "C:\\Program Files\\Microsoft Dynamics
NAV\\100\\Service\\Microsoft.Dynamics.Nav.Management.psm1"

### NavApp Cmdlets

The Apps.Management module imported above contains the cmdlets necessary for
working with extensions.

#### Publishing Extensions

The first step in applying an extension to a database is to ‘publish’ it. This
process verifies that there is no development in the extension that is
incompatible with the target database and makes the functionality available to
install in the target database.

The cmdlet to use is *Publish-NavApp*, as follows:

Publish-NavApp DynamicsNAV100 -Path “C:\\Extensions\\Promotions.navx”

Replace ‘DynamicsNAV100’ with the name of the server instance you are targeting
and the file path with the location of the navx file.

#### Installing Extensions

Once an extension has been published to a database it is available to install
into one or more of the tenants served by that database. The cmdletto use is
*Install-NavApp,* as follows:

Install-NavApp DynamicsNAV100 -Name ‘Promotions’ -Tenant ‘TenantA’

Replace ‘DynamicsNAV100’ with the name of the server instance you are targeting,
‘Promotions’ with the name of the extension and ‘TenantA’ with the name of the
tenant you wish to install the extension into. If your database contains only a
single tenant the -Tenant switch can be omitted.

You can use the *Get-NavAppInfo* cmdlet to view details, including the name,
version and publisher of extensions that are published into the database.

#### Synchronising the Tenant

Having published and installed the extension it is good practice to check that
the metadata of the tenant is in sync with the SQL database. Use the
*Sync-NavTenant* cmdlet:

Sync-NAVTenant DynamicsNAV100 -Tenant ‘TenantA’

Again, replace DynamicsNAV100 with the name of the server instance you are
targeting and ‘TenantA’ with the name of the tenant. The -Tenant switch can be
omitted if the database contains only a single tenant.

#### Removing Extensions

Extensions can easily be removed from a database by reversing the installation
process.

Uninstall-NavApp DynamicsNAV100 -Name ‘Promotions’  
Unpublish-NavApp DynamicsNAV100 -Name ‘Promotions’

Replace ‘DynamicsNAV100’ and ‘Promotions’ as appropriate. [Synchronise the
tenant](#synchronising-the-tenant) afterwards.

Extension Management
--------------------

Extensions can also be managed through the NAV client. Open the “Extension
Management” page to view available extensions, install and uninstall them. Note
that only published extensions are visible in this page. [PowerShell must be
used to publish an extension](#publishing-extensions) before the Extension
Management page can be used to install it.
