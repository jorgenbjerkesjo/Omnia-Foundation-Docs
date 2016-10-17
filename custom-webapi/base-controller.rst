Omnia base controller 
============================

When building Web API for Omnia extension it's recommended to use the base class **SharePointContextProvidedController** from Omnia. This base class provides many useful services when working with SharePoint and Omnia.


.. code-block:: C#

    using Omnia.Foundation.Extensibility.Core;
    using Omnia.Foundation.Extensibility.Core.Utilities;
    using Omnia.Foundation.Extensibility.WebApi;
    using Omnia.Foundation.Models.Shared;
    using Omnia.Foundation.Models.Logging;
    using System;
    using System.Web.Http;
    using System.Collections.Generic;
    using Microsoft.SharePoint.Client;
    
    public class MyController: SharePointContextProvidedController
    {
        [HttpGet, Route("api/items")]
        public ApiOperationResult<IEnumerable<Item>> GetItems()
        {
            try
            {
               // Your controller logic here 
            }
            catch (Exception ex)
            {
                this
                    .Logging
                    .AddLog(this.GetType().ToString(), ex.Message, DefaultLogTypes.Error, ex);

                return ApiUtils.CreateErrorResult<IEnumerable<Item>>(ex);
            }
        }
    }

SharePoint ClientContext
--------------------------------------------------

The base class **SharePointContextProvidedController** will enforce the parameters **SPUrl** and **TokenKey** on every endpoints in the controller, either in querystring or in the request headers. These paramters will be used to authenticate who is calling the API and create a SharePoint user ClientContext. This client context can be accessed using the base controller's property **Ctx**. This way the Web API developers will not need to think about authentication and how to communicate with Omnia Foundation services.

The base controller class also provides a helper method, named **CreateContextFor**, to create ClientContext for another SharePoint site than the site specified by SPUrl paramter, or to create ClientContext with elevated permission. 

.. code-block:: C#

    using Omnia.Foundation.Extensibility.Core;
    using Omnia.Foundation.Extensibility.Core.Utilities;
    using Omnia.Foundation.Extensibility.WebApi;
    using Omnia.Foundation.Models.Shared;
    using Omnia.Foundation.Models.Logging;
    using System;
    using System.Web.Http;
    using System.Collections.Generic;
    using Microsoft.SharePoint.Client;
    
    public class MyController: SharePointContextProvidedController
    {
        [HttpGet, Route("api/items")]
        public ApiOperationResult<IEnumerable<Item>> GetItems()
        {
            try
            {
               // Use the current ClientContext               
               this.Ctx.Load(this.Ctx.Web, w => w.Url);
               thix.Ctx.ExecuteQuery();

               // Create ClientContext with elevated permission
               using (ClientContext appContext = CreateContextFor(this.Ctx.Web.Url, elevated: true)) 
               {

               }
            }
            catch (Exception ex)
            {
                this
                    .Logging
                    .AddLog(this.GetType().ToString(), ex.Message, DefaultLogTypes.Error, ex);

                return ApiUtils.CreateErrorResult<IEnumerable<Item>>(ex);
            }
        }
    }

To Create ClientContext with elevated permission you need to have a valid ClientId and ClientSecret in web.config (for Office 365) or have a high-trust certificate configured (for on-premise).    

.. code-block:: xml

    <appSettings>
        <add key="ClientSecret" value="DljsSY31wlsZFytMRhjr4xE11NynROTf0K1p/9XDWnM=" />
        <add key="ClientId" value="62669ea8-15f0-4b24-83e4-1e99fce5ecd5" />
        ...
    </appSettings>



Omnia Foundation Services
--------------------------------------------------

SharePointContextProvidedController has a built-in Logging service which write to Omnia Foundation logs database. Other Omnia Foundation services can be accessed using the factory method **WorkWith()** 

.. code-block:: C#

    using Omnia.Foundation.Extensibility.Core;
    using Omnia.Foundation.Extensibility.Core.Utilities;
    using Omnia.Foundation.Extensibility.WebApi;
    using Omnia.Foundation.Extensibility.Core.Configurations;
    using Omnia.Foundation.Models.Shared;
    using Omnia.Foundation.Models.Logging;
    using System;
    using System.Web.Http;
    using System.Collections.Generic;
    using Microsoft.SharePoint.Client;
    
    public class MyController: SharePointContextProvidedController
    {
        [HttpGet, Route("api/items")]
        public ApiOperationResult<IEnumerable<Item>> GetItems()
        {
            try
            {
               // Use Omnia configuration service
               var configuration = WorkWith().Configurations().GetConfiguration(
                        name: "configuration-name", 
                        region: "configuration-region");              
            }
            catch (Exception ex)
            {
                // Built-in logging service
                this
                    .Logging
                    .AddLog(this.GetType().ToString(), ex.Message, DefaultLogTypes.Error, ex);

                return ApiUtils.CreateErrorResult<IEnumerable<Item>>(ex);
            }
        }
    }

Other contextual information are also provided:

- **TenantId**: ID of the Omnia tenant that the current SharePoint site belongs to.
- **LoginName**: SharePoint loginname of the current user.
- **OmniaInstanceMode**: The mode that Omnia Foundation is running in, either Tenant or SiteCollectionOnly