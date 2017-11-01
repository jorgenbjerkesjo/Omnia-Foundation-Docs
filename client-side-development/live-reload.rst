Live reload
=============================

When doing client side development, you can enable a mode called "Live reload". In this mode, the page is automatically updated to reflect your new HTML, TypeScript and Less code when you save a file in Visual Studio.

.. contents:: Sections:
  :local:
  :depth: 1

Enable Live reload
##########

1. Open Visual Studio as Administrator

2. In the **environment.json** file of your Omnia extension, do the following

 - In the *Hosting* section set *Enabled* to **true**, and *UseLiveReload* to **true**
 - Note the **Port** number (ex 9900)
 - Save the file
 
 .. image:: /images/live-reload-environment-json.png
 
3. In the **Output** window of Visual Studio, select "Omnia Tooling" in the *Show output from* dropdown

4. Make sure the Hosting server has started correctly by reading the output. 

 .. image:: /images/live-reload-output-window.png
 
.. note:: If you get an error message in the output, you need to make sure that the IIS Express SSL certificate is installed on your computer. 
 
  You can trigger the installation by creating an MVC project, Enable SSL and then start Debugging the project. You will now get a dialog asking if you want to trust the IIS Express SSL certificate. Click "Yes"

3. In your web browser

 - Navigate to a site in the Office 365 Tenant used for developing your Omnia Extension
 - Add **?console=on** to the URL of the page, ex */somepage?console=on*. 
   If you are on a quick page, instead add **|console=on**, ex */#/somepage|console=on*
 - This will display the Omnia Console on the current page. Type **help** in the console to get a list of all available commands
 - To enable Live reload, type **hosting enable https://localhost:9900** where 9900 is the port number you noted in Step 2, from the environment.json
  .. image:: /images/live-reload-omnia-console-enable.png
 
 - The page should now be reloaded and after some time your controls should be displayed, now served from **https://localhost:9900** instead of the Omnia server

Working in Live reload mode
##########

Edit your LESS, TypeScript or HTML files as you normally would. When saved, the browser will update to instantly display the changes.



Disable Live reload
#########

To disable Live reload, do the following:

1. In the Omnia console in the browser window, write **hosting disable**
2. In the Visual Studio project, in the environment.json file, set the *Enabled* parameter in the *Hosting* section to **false**
    

    
    