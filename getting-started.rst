Getting Started
===============

1. Download and install `Omnia Tooling for Visual Studio <#>`_
##############################################################

2. Create new project with Omnia Extension project template
##############################################################

.. image:: /images/toolings-project-templates.png

.. note:: 
    - **Omnia Documentation** - Project template for Omnia documentation package
    - **Omnia Extension** - Project template for empty Omnia extension package
    - **Omnia Extension Sample** - Project template for Omnia extension package and Web API with samples
    - **Omnia Extension With Web API** -  Project template for empty Omnia extension package and Web API

A project created with Omnia Extension Sample template will have a stucture like this

.. image:: /images/toolings-project-structure.png

3. Register your extension in Omnia Foundation
##############################################################

Open the file **extension.json** in MyOmniaExtension project and copy the extension ID

.. image:: /images/toolings-extension-id-json.png

Go to the admin page of your Omnia environment and navigate to **System > Extensions > Register Extension**

.. image:: /images/omnia-admin-register-extension.png

Fill in your extension ID and click create. Your extension will be registered with a secret key which you should save for later use.

.. image:: /images/omnia-admin-new-extension-secret.png

4. Set the environment information in your project
##############################################################

Open the file **environment.json** in MyOmniaExtension  and fill in:

- TenantId: You can get from the System page in Omnia admin
- ApiSecret: The secret you get when registered your extension in step 3
- FoundationUrl: the URL to your Omnia admin 

.. image:: /images/toolings-environment-json.png

5. Deploy your extension
##############################################################

Right click on MyOmniaExtension project and click Omnia

.. image:: /images/toolings-omnia-deploy.png

You can see the deployment progress in the Output window in Visual Studio

.. image:: /images/toolings-omnia-deploy-output.png 

