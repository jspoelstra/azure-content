<properties linkid="dev-nodejs-how-to-powershell" urldisplayname="PowerShell Cmdlets" headerexpose pagetitle="How to Use the Windows Azure PowerShell for Node.js" metakeywords="Azure PowerShell Node.js, Azure PowerShell Node.js cmdlet" footerexpose metadescription="Learn Windows PowerShell fundamentals and details about how to use the Windows Azure PowerShell for Node.js cmdlets." umbraconavihide="0" disquscomments="1"></properties>

# How to Use Windows Azure PowerShell for Node.js

This guide describes how to use Windows PowerShell cmdlets to create,
test, deploy, and manage Node.js services in Windows Azure. The
scenarios covered include **importing your publishing settings**,
**creating Windows Azure services to host Node.js applications**,
**running a service in the Windows Azure compute emulator**, **deploying
and updating hosted services**, **setting deployment options for a
service**, and **stopping, starting, and removing a service**.

**Note** For a detailed description of each Node.js cmdlet, see the
[Windows Azure PowerShell for Node.js Cmdlet Reference][].

## Table of Contents

[What is Windows Azure PowerShell for Node.js][]   
 [Get Started Using Windows Azure PowerShell for Node.js][]   
 [How to: Import Publishing Settings][]   
 [How to: Create a Windows Azure Service][]   
 [How to: Test a Service Locally in the Windows Azure Emulators][]   
 [How to: Set Default Deployment Options for a Service][]   
 [How to: Use a Storage Account with More than One Service][]   
 [How to: Deploy a Hosted Service to Windows Azure][]   
 [How to: Update a Deployed Service][]   
 [How to: Scale Out a Service][]   
 [How to: Stop, Start, and Remove a Service][]

## <a id="_What_Is_Windows" name="_What_Is_Windows"> </a>What Is Windows Azure PowerShell for Node.js

Windows Azure PowerShell for Node.js provides a command-line environment
for developing and deploying Node applications for Windows Azure through
a few Windows PowerShell cmdlets.

The following tasks are supported:

-   Import publishing settings to enable you to deploy services in
    Windows Azure.
-   Generate configuration files and a sample application for a Node
    hosted service. Create a Windows Azure service that contains web
    roles and worker roles.
-   Test your service locally using the Windows Azure compute emulator.
-   Deploy your service to the Windows Azure staging or production
    environment.
-   Scale and update services in Windows Azure.
-   Enable and disable remote access to service role instances.
-   Start, stop, and remove services.

## <a id="_Get_Started_Using" name="_Get_Started_Using"> </a>Get Started Using Windows Azure PowerShell for Node.js

For requirements and installation instructions for Windows Azure
PowerShell for Node.js, see the [Node.js Web Application][] tutorial.

### Getting Started Using Windows PowerShell

If you have not used Windows PowerShell before, the following resources
can help you get started:

-   For basic instructions, see [Using Windows PowerShell][] in the
    [Windows PowerShell Getting Started Guide][].

-   While you are working in Windows PowerShell, your best source of
    help is the **Get-Help** cmdlet. The following table summarizes some
    common help requests. For more information, see [Getting Help:
    Get-Help][], or, in Windows PowerShell, type: **get-help**.

    <table border="1" cellspacing="4" cellpadding="4">
    <tbody>
    <tr align="left" valign="top">
    <td>
    **Cmdlet Format**

    </td>
    <td>
    **Information Returned**

    </td>
    </tr>
    <tr align="left" valign="top">
    <td>
    get-help

    </td>
    <td>
    Displays a help topic about using the **Get-Help** cmdlet

    </td>
    </tr>
    <tr align="left" valign="top">
    <td>
    get-help azure

    </td>
    <td>
    Lists all cmdlets in the Windows Azure Node.js snap-in

    </td>
    </tr>
    <tr align="left" valign="top">
    <td>
    get-help <*cmdlet*\>

    </td>
    <td>
    Displays help about a Windows PowerShell cmdlet

    </td>
    </tr>
    <tr align="left" valign="top">
    <td>
    get-help <*cmdlet*\> -parameter \*

    </td>
    <td>
    Displays parameter definitions for a cmdlet

    </td>
    </tr>
    <tr align="left" valign="top">
    <td>
    get-help <*cmdlet*\> -examples

    </td>
    <td>
    Displays example syntax lines for a cmdlet

    </td>
    </tr>
    <tr align="left" valign="top">
    <td>
    get-help <cmdlet\> -full

    </td>
    <td>
    Displays technical requirements for a cmdlet

    </td>
    </tr>
    </tbody>
    </table>

### Getting Started Using Windows Azure PowerShell for Node.js

The Node.js cmdlets have a few special requirements that are not common
to all Windows PowerShell components:

-   To deploy your Node applications in Windows Azure, you must have a
    Windows Azure subscription. Before you can deploy Node applications
    by using a Node.js cmdlet, you must download your subscription
    information (by using **Get-AzurePublishSettings**) and then import
    those settings (by using **Import-AzurePublishSettings**).

-   You must run cmdlets that act on a hosted service from within the
    service directory.

    When you create a new hosted service, a service directory is created
    in the current directory, and the focus of the Windows PowerShell
    command prompt moves to the service directory. From the service
    directory, you can add web roles and worker roles to the service.
    All other cmdlets for the service can be run from any child
    directory of the service directory.

-   After you create and configure a new hosted service, or after you
    run any cmdlet that updates the configuration of a deployed service,
    you must run the **Publish-AzureService** cmdlet to publish the
    updates to the cloud service deployment. For example, after you run
    **Set-AzureInstances** to add additional web role instance to a
    service configuration, run
    <a id="_GoBack" name="_GoBack"></a>**Publish-AzureService** to scale
    out the hosted service.

    If you are running the deployment locally in the Windows Azure
    compute emulator, you must run **Start-AzureEmulator** again after
    you update the service definition file (.csdef) or the [service
    configuration] file (.cscfg). However, the compute emulator renders
    updates to the server.js and web.config files instantly.

-   Although Windows Azure PowerShell cmdlets and parameters are not
    case-sensitive, the following values that are entered for Node.js
    cmdlets are case-sensitive: service names, subscription names,
    storage account names, and deployment locations.

### To open Windows Azure PowerShell for Node.js

-   On the **Start** menu, click **All Programs**, click **Windows Azure
    SDK for Node.js**, and then click **Windows PowerShell for
    Node.js**. Opening your Windows PowerShell environment this way
    ensures that all of the Node command-line tools are available.

### Example Syntax Lines

In the example syntax lines in this guide, all Node.js services are
created from a C:\\node folder. A C:\\node folder is not required; you
can create your Windows Azure services from any location. Most example
syntax lines use a service named MyService, and cmdlets performed on the
service are entered at the following command prompt:

    C:\node\MyService> █

## <a id="_How_to_Import" name="_How_to_Import"> </a>How to: Import Publishing Settings

To deploy your Node applications in Windows Azure, you must have a
Windows Azure subscription. If you do not have a Windows Azure
subscription, see [purchase options][] for Windows Azure for
information.

Before you can deploy Node applications by using a Node.js cmdlet, you
must download your subscription information (by using
[Get-AzurePublishSettings][]) and then import those settings (by using
[Import-AzurePublishSettings][]).

The **Get-AzurePublishSettings** cmdlet opens a web page on the
[Microsoft Online Services Customer Portal][] from which you can
download the publishing profile. You will need to log on to the Customer
Portal using the credentials for your Windows Azure account.

**Note** For information about the contents of the publishing profile,
see the **Get-AzurePublishingSettings** cmdlet in the [Windows Azure
PowerShell for Node.js Cmdlet Reference][].

When you download the publishing profile, note the path and the name of
your settings file. You must provide this information when you use
**Import-AzurePublishSettings** to import the settings. The default
location and file name format is:

C:\\Users\\<MyAccount\>\\Downloads\\[*MySubscription*-…]-*downloadDate*-credentials.publishsettings

The following example shows how to download publishing settings for your
Windows Azure account.

    Get-AzurePublishSettings

In the following example, publishing settings that were downloaded to
the default path on 11-11-2011 are imported. In this case, the user is a
co-administrator for the Project1 subscription in addition to his own
subscription.

    Import-AzurePublishSettings C:\Users\MyAccount\Downloads\MySubscription-Project1-11-11-2011-credentials.publishsettings

If, after you import your publish settings, you are added to other
subscriptions as a co-administrator, you will need to repeat this
process to download a new .publishsettings file, and then import those
settings. For information about adding co-administrators to help manage
services for a subscription, see [How to Add and Remove
Co-Administrators for Your Windows Azure Subscription][].

**Important** You should delete the publishing profile that you
downloaded using **Get-AzurePublishSettings** after you import those
settings. The downloaded profile contains a management certificate that
should not be accessed by unauthorized users. If you need information
about your subscriptions, you can get it from the [Windows Azure
Platform Management Portal][] or the [Microsoft Online Services Customer
Portal][].

## <a id="_How_to_Create" name="_How_to_Create"> </a>How to: Create a Windows Azure Service

Use the [New-AzureService][] cmdlet to create the scaffolding for a
hosted service for your Node.js application.

The following example shows how to create a new hosted service named
MyService.

    PS C:\node> New-AzureService MyService

The cmdlet creates a service subdirectory on your local computer, adds
service configuration files to the service directory, and changes the
focus of the Windows PowerShell command prompt to the new service
directory.

After you create the service, you can run [Add-AzureNodeWebRole][] or
[Add-AzureNodeWorkerRole][] from the service directory to configure a
web role or worker role for the service.

When your application is deployed as a hosted service in Windows Azure,
it runs as one or more *roles.* A *role* simply refers to the
application files and configuration. You can define one or more roles
for your application, each with its own set of application files and its
own configuration. A web role is customized for web application
programming, while a worker role is intended to support general
development and periodic or long-running processes. For more information
about service roles, see [Overview of Creating a Hosted Service for
Windows Azure][].

You can run either of these cmdlets with no parameters to create a
single role instance with the name WebRole1 or WorkerRole1. Use the
**-Name** parameter to use a different role name.

For each role in your application, you can specify the number of virtual
machines, or *role instances*, to deploy using the **-Instances**
option.

The following example shows how to use the **Add-AzureNodeWebRole**
cmdlet to create a new web role named **MyWebRole** that has two
instances.

    PS C:\node\MyService> Add-AzureNodeWebRole MyWorkerRole -I 2

## <a id="_How_to_Test" name="_How_to_Test"> </a>How to: Test a Service Locally in the Windows Azure Emulators

The [Start-AzureEmulator][] cmdlet starts the service in the Windows
Azure compute emulator and also starts the Windows Azure storage
emulator. You can use the compute emulator to test the service locally
before you deploy the service to Windows Azure. You can use the storage
emulator to test storage locally before your application consumes
Windows Azure storage services.

If your application includes a web role, you can use the **–Launch**
parameter to open the web role in a browser.

The following example runs the MyService application in the compute
emulator and opens the web role in a browser.

    PS C:\node\MyService> Start-AzureEmulator -Launch

After you finish testing an application locally, run the
[Stop-AzureEmulator][] cmdlet to stop the Windows Azure compute
emulator, as shown below.

The following example shows how to use **Stop-AzureEmulator** to stop
the emulator.

    PS C:\node\MyService> Stop-AzureEmulator

## <a id="_How_to_Set" name="_How_to_Set"> </a>How to: Set Default Deployment Options for a Service

You can use the [Set-AzureDeploymentLocation][],
[Set-AzureDeploymentSlot][], [Set-AzureDeploymentSubscription][], and
[Set-AzureDeploymentStorage][] cmdlets to set the default deployment
location, slot (Staging or Production), Windows Azure subscription, and
storage account to use when you deploy a service. The default options
apply to an individual service. You can run the cmdlets from anywhere in
the service directory.

These options take effect when you next deploy the service (using
**Publish-AzureService**). If you want to override a default deployment
option during a service deployment, you can use a parameter for the
**Publish-AzureService** cmdlet.

If you have not set a default deployment option and you do not specify a
deployment option to use when you publish the service, the service is
deployed using the following settings:

<table border="1" cellspacing="4" cellpadding="4">
<tbody>
<tr align="left" valign="top">
<td valign="bottom">
**Setting**

</td>
<td valign="bottom">
**Default Value**

</td>
</tr>
<tr align="left" valign="top">
<td>
Location

</td>
<td>
Randomly assigns the service to either South Central US or North Central
US.

</td>
</tr>
<tr align="left" valign="top">
<td>
Slot

</td>
<td>
Deploys the service to a production slot.

</td>
</tr>
<tr align="left" valign="top">
<td>
Subscription

</td>
<td>
Uses the first subscription in your publishing profile. If you are an
administrator for more than one subscription, you should specify a
subscription to ensure that the intended subscription is used.

</td>
</tr>
<tr align="left" valign="top">
<td>
Storage account

</td>
<td>
Creates a new storage account that has the same name as the service, If
the name has been used for a storage account for any other subscription,
the deployment fails. In that case, you must specify a storage account
to use for the service.

</td>
</tr>
</tbody>
</table>
In the following example, the default deployment location for the
MyService service is set to Southeast Asia:

     PS C:\node\MyService> Set-AzureDeploymentLocation "Southeast Asia"

By default a service is published to a production slot, where it is
assigned a friendly URL based on the service name
(http://*MyService*.cloudapp.net). If you prefer to deploy the service
to a staging slot for testing before you deploy to production, you can
set the default deployment slot to Staging.

The following example sets the default deployment slot for the MyService
service to Staging.

    PS C:\node\MyService> Set-AzureDeploymentSlot -Slot Staging

You only need to set a deployment subscription for a service if you are
an administrator for more than one Windows Azure subscription. If you
have been assigned as a co-administrator for subscriptions other than
your own subscription, use the **Set-AzureDeploymentSubscription**
cmdlet to specify which subscription to use for a service.

The following example sets the ContosoFinanace subscription as the
default subscription to use for the MyService service.

    PS C:\node\MyService> Set-AzureDeploymentSubscription Contoso_Finance

## <a id="_How_to_Use" name="_How_to_Use"> </a>How to: Use a Storage Account with More Than One Service

When you deploy a new service, by default, a new storage account is
created in the deployment location, and the application package and
configuration files are copied to the Windows Azure Blob service using
that storage account. The new storage account has the same name and
location as the service, and it is associated with the subscription that
was used to deploy the service.

If you want to use an existing storage account with a service, you can
use the [Set-AzureDeploymentStorage][] cmdlet to specify that storage
account as the default storage account for service deployments, or you
can use the **-Storage** parameter for **Publish-AzureService** to
specify the storage account for the current service deployment.

To find out which storage accounts are available for your Windows Azure
subscription, run the [Get-AzureStorageAccounts][] cmdlet. If you are a
co-administrator for more than one subscription, use the
**-Subscription** parameter to specify which subscription to retrieve
the storage information for. The cmdlet retrieves the storage account
name and access keys for each storage account.

**Note** For information about creating, managing, and deleting storage
accounts, [How to: Manage Storage Accounts for a Windows Azure
Subscription][].

In the following example, a service co-administrator retrieves storage
account information for the ContosoFinance subscription.

     PS C:\ > Get-AzureStorageAccounts -Subscription ContosoFinance

    Account Name: ContosoUS
    Primary Key: YSAwVSjixHpcsK/IX7cRcqzVVa19YCUEhzndhZMZL9aMmNT2Du1DPiufPDBiJUO7FW4Dcb7tkzw14VoK0EppnA==
    Secondary Key: OBlsaR6A4untNNwuhHDkWkcI7pKwTEPA9JYO/Jv2m/zERqrtMjUGVpz8xRZ2mTPp5qksu9K2JawAo5rEKDaL+w==
    Account Name: ContosoAsia
    Primary Key: OzAqwcrrtHa4/5qUyekSRK1F257PrzQHE+i4TJc38MHDBDNjZesbbftfm5tta2rsNH0SM7DEnlqt9PW70AB1VA==
    Secondary Key: xjCQHNwgedo/RXMOk1PKqRHiEpox001/H+qgl/OphoKzOoQTzR/FAGGobsf5HgjE35lfPAD0KeApGFv4ga0hhw==

The co-administrator then sets ContosoUS as the default storage account
to use for the MyService service, running the cmdlet from the MyService
service directory.

    PS C:\node\MyService> Set-AzureDeploymentStorage -StorageAccountName ContosoUS

## <a id="_How_to_Deploy" name="_How_to_Deploy"> </a>How to: Deploy a Hosted Service to Windows Azure

When you are ready to deploy your service to Windows Azure, use the
[Publish-AzureService][] cmdlet. When you deploy a new hosted service,
Windows Azure performs the following tasks:

1.  Packages the source and configuration files into a service package
    (.cspkg file).

2.  Creates a new hosted service.

3.  If no storage account is specified, creates a new storage account if
    needed.

4.  Copies the service package and configuration files to the Windows
    Azure blob store for the storage account.

5.  Creates the hosted service and deployment using the uploaded service
    package.

Use the following parameters to specify deployment options for the
current deployment.

### Changing the service name

The service name must be unique within Windows Azure. If the name you
gave the service when you created it is not unique, you can use the
**-Name** parameter to assign a new name.

In the following example, the service name is changed to MyService01
when the service is deployed. The name of the service directory does not
change, but the service will be known in Windows Azure as MyService01.

    PS C:\node\MyService> Publish-AzureService -Name MyService01

**Note** If you specify a service name that is not unique in Windows
Azure, the service deployment fails, and you will see the following
error: "Publish-AzureService : The remote server returned an unexpected
response: (409) Conflict."

### Setting a subscription for this deployment

You only need to use the **-Subscription** parameter if you are an
administrator for more than one Windows Azure subscription. In that
case, it is recommended that you include this parameter to ensure that
you use the intended subscription with the service.

In the following example, the Contoso\_Finance subscription is used to
deploy the MyService service.

    PS C:\node\MyService> Publish-AzureService -Subscription Contoso_Finance
      

### Specifying the location for this deployment

Use the **-Location** parameter to specify a geographic region for the
service deployment. If you have not set a default deployment location
for the service, and you do not specify a location using this parameter,
the service is assigned randomly to either to North Central US or South
Central US.

To get a list of available locations, run the following cmdlet.

    Get-Help Publish-AzureService -Parameter location 

      -Location <String>
      The region in which the application will be hosted. Possible values are: An
      ywhere Asia, Anywhere Europe, Anywhere US, East Asia, North Central US, Nor
      th Europe, South Central US, Southeast Asia, West Europe. If no Location i
      s specified, the location specified in the last call to Set-AzureDeployment
      Location will be used. If no Location was ever specified, the Location wil
      l be randomly chosen from ou include this parameter to ensure that you use the intended subscription with the service.

In the following example, the Contoso\_Finance subscription is used to
deploy the MyService service.

    PS C:\node\MyService> Publish-AzureService -Subscription Contoso_Finance
      

### Specifying the location for this deployment

Use the **-Location** parameter to specify a geographic region for the
service deployment. If you have not set a default deployment location
for the service, and you do not specify a location using this parameter,
the service is assigned randomly to either to North Central US or South
Central US.

To get a list of available locations, run the following cmdlet.

     Get-Help Publish-AzureService -Parameter location 

      -Location <String>
      The region in which the application will be hosted. Possible values are: An
      ywhere Asia, Anywhere Europe, Anywhere US, East Asia, North Central US, Nor
      th Europe, South Central US, Southeast Asia, West Europe. If no Location i
      s specified, the location specified in the last call to Set-AzureDeployment
      Location will be used. If no Location was ever specified, the Location wil
      l be randomly chosen from 'North Central US' and 'South Central US' locatio
      ns.
      

The **-Location** value is case-sensitive, and, because the location
values have multiple words, you must enclose the location in quotation
marks.

In the following example, the MyService service is deployed to the
Anywhere US location.

    PS C:\node\MyService> Publish-AzureService -Location "Anywhere US"

### Using an existing storage account

If you manage multiple services in the same location, you can include
the **-StorageAccountName** parameter to assign an existing storage
account in that location instead of creating a new storage account for
the service. If you have not specified a default storage account for the
service, and you do not specify a service account when you deploy the
service, a new storage account is created, even if an existing storage
account is available in that location. For information about creating
and managing storage accounts, see [How to: Manage Storage Accounts for
a Windows Azure Subscription][].

In the following example, the MyService service is deployed using the
StorageUS storage account for the ContosoFinance subscription.

    PS C:\node\MyService> Publish-AzureService -Subscription ContosoFinance -StorageAccountName StorageUS

### Deploying a service to staging or production

Use the **-Slot** parameter to specify whether to deploy the service to
the staging environment or the production environment in Windows Azure.
By default, services are deployed to production. In Windows Azure, the
staging and production environments are distinguished by the address
that is used to access the service. Use the staging environment to test
a service before deploying it to production, where a friendlier URL is
assigned:

-   Staging URL format: <ServiceID\>.cloudapp.net  
     Example: http://b8f3dd0c084a4add81e1c3345eb0af87.cloudapp.net/

-   Production URL format: <ServiceName\>.cloudapp.net  
     Example: http://MyService.cloudapp.net

In the following example, the MyService service is deployed to the
Windows Azure staging environment.

    PS C:\node\MyService> Publish-AzureService -Slot Staging

**Note** For more information about managing staging and production
deployments, see [Overview of Managing Deployments in Windows Azure][].

### Opening the web role in a browser

If the service contains a web role, you can include the **-Launch**
parameter to open the web role in a browser. All services are started
automatically after they are deployed, but the web role is not opened
unless the **-Launch** parameter is included.

### Examples

In the following example, the MyService service is deployed using values
that have been set previously. A new storage account is created. The
service is deployed to the production environment.

    PS C:\node\MyService> Publish-AzureService

    Publishing to Windows Azure. This may take several minutes...
    6:15:58 PM - Preparing deployment for MyService with Subscription ID: 0807028c-e0a5-4773-82e3-8cae71dd5702...
    6:16:04 PM - Connecting...
    6:16:07 PM - Creating...
    6:16:09 PM - Created hosted service 'MyService'.
    6:16:09 PM - Verifying storage account 'myservice'...
    6:18:14 PM - Uploading Package...
    6:20:41 PM - Created Deployment ID: 7ce799c2023a4ae9b346b970045cd14c.
    6:20:41 PM - Starting...
    6:20:41 PM - Initializing...
    6:20:55 PM - Instance WebRole1_IN_0 of role WebRole1 is creating the virtual machine.
    6:24:26 PM - Instance WebRole1_IN_0 of role WebRole1 is busy.
    6:25:42 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    6:25:43 PM - Created Website URL: http://MyService.cloudapp.net.
    6:25:43 PM - Complete.

In the following example, parameters are used to set the subscription
(ContosoFinance), slot (Staging), and location (North Central US) for
the current service deployment.

    PS C:\node\MyService> Publish-AzureService -Subscription ContosoFinance -Sl staging -L "North Central US"

## <a id="_How_To:_Update" name="_How_To:_Update"> </a>How To: Update a Deployed Service

When you use

[Publish-AzureService][] on a deployed service, the service is either
updated in place or a new deployment is created. The update method
depends on the types of changes that you make.

The following types of update are performed in place, without
redeploying the service:

-   Adding a new web role or worker role to the service, or changing the
    number of instances of an existing role.

-   Updating the application or the service configuration file.

-   Enabling or disabling remote access to service role instances.

The following types of update initiate a new service deployment:

-   If you change the subscription, storage account, or deployment
    location, a new service deployment occurs, and the previous
    deployment is deleted.

-   If you change the deployment slot - for example, you deploy a
    service that is in the staging environment to the production
    environment - a second deployment is performed without deleting the
    first deployment.

In the following example, the MyService service is updated in place
after a second service role instance is added to the WebRole1 service
role.

    PS C:\node\MyService> Update-AzureInstances MyWebRole 2
    PS C:\node\MyService> Publish-AzureService

The following example shows how you would publish a new service
deployment to production. If a service deployment exists in staging,
both deployments are retained.

    PS C:\node\MyService> Publish-AzureService -Slot Production

In the following example, a service that has been deployed to the
Anywhere Europe location is redeployed to North Europe. Because the
location is changing, the service is redeployed and the existing
deployment is removed. An existing storage account for the North Europe
location is used.

    PS C:\node\MyService> Publish-AzureService -Location "North Europe" -StorageAccountName NorthEuropeStore

## <a id="_How_to:_Scale" name="_How_to:_Scale"> </a>How to: Scale Out a Service

After you deploy your service, you can run the [Set-AzureInstances][]
cmdlet to scale the service out or in by adding or removing instances to
a web role or a worker role.

The following example shows how to update the MyService service
configuration to deploy two instances of the existing web role named
MyWebRole when you next publish the service.

    PS C:\node\MyService> Update-AzureInstances MyWebRole 2

To deploy the change to the role instances, you must run the
[Publish-AzureService][] cmdlet. Note that a change to the number of
role instances causes the virtual machines to be rebuilt, and you will
lose any data that is stored locally on the role instances.

## <a id="_How_to:_Stop," name="_How_to:_Stop,"> </a>How to: Stop, Start, and Remove a Service

A deployed application, even if it is not running, continues to accrue
billable time for your subscription. Therefore, it is important that you
remove unwanted deployments from your Windows Azure subscription. You
also have the option to stop your service but not remove it. However, if
you do not remove the service, you will still accrue charges for the
compute units (virtual machines), even if the service is stopped.

Use the [Stop-AzureService][] cmdlet to stop a running, deployed
service. If you have deployments in both staging and production, you can
use the -**Slot** parameter to specify which deployment to stop. If you
do not specify a slot, both deployments are stopped.

The following example shows how to stop the MyService service.

    PS C:\node\MyService> Stop AzureService

Use the [Start-AzureService][] cmdlet to restart a service that is
stopped. For a service with both staging and production deployments, the
**-Slot** parameter specifies which deployment to start. If the
**-Slot** parameter is not included, both deployments are started.

The following example shows how to start the production deployment of
the MyService service.

    PS C:\node\MyService> Start-AzureService -Slot production

Use the [Remove-AzureService][] cmdlet to remove a service. If the
service is running, the service is stopped and then removed. If you have
service deployments in both staging and production, this cmdlet removes
both deployments.

The following example removes the MyService service.

    PS C:\node\MyService> Remove-AzureService

## Additional Resources

[Windows Azure PowerShell for Node.js Cmdlet Reference][]   
 [Node.js Web Application][1]   
 [Node.js Web Application with Table Storage][]   
 [Enabling Remote Desktop in Windows Azure][]   
 [Configuring SSL for a Node.js Application in Windows Azure][]

  [Windows Azure PowerShell for Node.js Cmdlet Reference]: http://msdn.microsoft.com/en-us/library/windowsazure/hh689725(vs.103).aspx
  [What is Windows Azure PowerShell for Node.js]: #_What_Is_Windows
  [Get Started Using Windows Azure PowerShell for Node.js]: #_Get_Started_Using
  [How to: Import Publishing Settings]: #_How_to_Import
  [How to: Create a Windows Azure Service]: #_How_to_Create
  [How to: Test a Service Locally in the Windows Azure Emulators]: #_How_to_Test
  [How to: Set Default Deployment Options for a Service]: #_How_to_Set
  [How to: Use a Storage Account with More than One Service]: #_How_to_Use
  [How to: Deploy a Hosted Service to Windows Azure]: #_How_to_Deploy
  [How to: Update a Deployed Service]: #_How_To:_Update
  [How to: Scale Out a Service]: #_How_to:_Scale
  [How to: Stop, Start, and Remove a Service]: #_How_to:_Stop,
  [Node.js Web Application]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/web-app-with-express/
  [Using Windows PowerShell]: http://msdn.microsoft.com/en-us/library/ms714409.aspx
  [Windows PowerShell Getting Started Guide]: http://msdn.microsoft.com/en-us/library/aa973757.aspx
  [Getting Help: Get-Help]: http://msdn.microsoft.com/en-us/library/bb648604.aspx
  [purchase options]: http://www.windowsazure.com/en-us/pricing/purchase-options/
  [Get-AzurePublishSettings]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757270(vs.103).aspx
  [Import-AzurePublishSettings]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757264(vs.103).aspx
  [Microsoft Online Services Customer Portal]: https://mocp.microsoftonline.com/site/default.aspx
  [How to Add and Remove Co-Administrators for Your Windows Azure
  Subscription]: http://msdn.microsoft.com/en-us/library/windowsazure/gg456328.aspx
  [Windows Azure Platform Management Portal]: http://windows.azure.com/
  [New-AzureService]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757269(vs.103).aspx
  [Add-AzureNodeWebRole]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757267(vs.103).aspx
  [Add-AzureNodeWorkerRole]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757254(vs.103).aspx
  [Overview of Creating a Hosted Service for Windows Azure]: http://msdn.microsoft.com/en-us/library/windowsazure/gg432976.aspx
  [Start-AzureEmulator]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757255(vs.103).aspx
  [Stop-AzureEmulator]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757258(vs.103).aspx
  [Set-AzureDeploymentLocation]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757261(vs.103).aspx
  [Set-AzureDeploymentSlot]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757252(vs.103).aspx
  [Set-AzureDeploymentSubscription]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757253(vs.103).aspx
  [Set-AzureDeploymentStorage]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757268(vs.103).aspx
  [Get-AzureStorageAccounts]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757259(vs.103).aspx
  [How to: Manage Storage Accounts for a Windows Azure Subscription]: http://msdn.microsoft.com/en-us/library/windowsazure/hh531567.aspx
  [Publish-AzureService]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757266(vs.103).aspx
  [Overview of Managing Deployments in Windows Azure]: http://msdn.microsoft.com/en-us/library/windowsazure/hh386336.aspx
  [Set-AzureInstances]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757263(vs.103).aspx
  [Stop-AzureService]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757256(vs.103).aspx
  [Start-AzureService]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757265(vs.103).aspx
  [Remove-AzureService]: http://msdn.microsoft.com/en-us/library/windowsazure/hh757257(VS.103).aspx
  [1]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/getting-started/
  [Node.js Web Application with Table Storage]: http://www.windowsazure.com/en-us/develop/nodejs/tutorials/web-app-with-storage/
  [Enabling Remote Desktop in Windows Azure]: http://www.windowsazure.com/en-us/develop/nodejs/common-tasks/enable-remote-desktop/
  [Configuring SSL for a Node.js Application in Windows Azure]: http://www.windowsazure.com/en-us/develop/nodejs/common-tasks/enable-ssl/