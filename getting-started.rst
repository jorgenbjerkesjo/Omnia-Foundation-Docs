Getting Started
===============

1. Install Node JS
######

Download and install Node JS from https://nodejs.org/ . Choose the latest **Current** version.

2. Install Office Developer Tools
####

Download and install Office Developer Tools from https://www.visualstudio.com/vs/office-tools/

3. Install TypeScript for Visual Studio
#####

Download and install TypeScript for Visual Studio. 

The installation file for Visual Studio 2015 is available from https://www.microsoft.com/en-us/download/details.aspx?id=48593

4. Install Omnia Tooling for Visual Studio
##############################################################

Download and install :doc:`Omnia Tooling for Visual Studio <release-notes/tooling>`

5. Create new project with Omnia Extension project template
##############################################################

.. image:: /images/toolings-project-templates.png

.. note:: 
    - **Omnia Documentation** - Project template for empty Omnia documentation package
    - **Omnia Extension** - Project template for empty Omnia extension package
    - **Omnia Extension Sample** - Project template for Omnia extension package and Web API with samples
    - **Omnia Extension With Web API** -  Project template for empty Omnia extension package and Web API

A project created with Omnia Extension Sample template will have a structure like this

.. image:: /images/toolings-project-structure.png

6. Configure Precio Fishbone NuGet package source in Visual Studio
##############################################################

Precio Fishbone NuGet package source is needed for download Omnia NuGet packages. To configure Precio Fishbone NuGet package source, follow these steps:

- In Visual Studio, navigate to **Tools > NuGet Packages Manager > Packages Manager Settings**

- In the Options dialog, select **Package Sources** in the left pane

- Add a new package source named Precio Fishbone with this URL `<http://nuget.preciofishbone.se/api/v2>`_

.. image:: /images/nuget-package-source.png


7. Register your extension in Omnia Foundation
##############################################################

Open the file **extension.json** in MyOmniaExtension project and copy the extension ID

.. image:: /images/toolings-extension-id-json.png

Go to the admin page of your Omnia environment and navigate to **System > Extensions > Register Extension**

.. image:: /images/omnia-admin-register-extension.png

Fill in your extension ID and click create. Your extension will be registered with a secret key which you should save for later use.

.. image:: /images/omnia-admin-new-extension-secret.png

8. Set the environment information in your project
##############################################################

Open the file **environment.json** in MyOmniaExtension  and fill in:

- TenantId: you can get from the System page in Omnia admin
- ApiSecret: the secret you got when registered your extension in step 3
- FoundationUrl: the URL to your Omnia admin 

.. image:: /images/toolings-environment-json.png

9. Deploy your extension
##############################################################

Right click on MyOmniaExtension project and click Omnia Deploy

.. image:: /images/toolings-omnia-deploy.png

You can see the deployment progress in the Output window in Visual Studio

.. image:: /images/toolings-omnia-deploy-output.png 

10. Verify 
##############################################################

After the extension has been deployed successfully to Omnia, you can verify it by navigating to **System > Extension** in Omnia admin

.. image:: /images/omnia-admin-new-extension-success.png 

And in the **Features** page you should see the features from your extension. Click on MyOmniaExtension Sample Feature Core and click Activate.

.. image:: /images/omnia-admin-new-extension-feature.png 

After the feature has been activate, refresh the page and you should see the hello world page from your extension:

.. image:: /images/omnia-admin-new-extension-helloworld.png 
