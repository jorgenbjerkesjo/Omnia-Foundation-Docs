Logging
============================

Omnia provides a logging API for extensions to write logs to Omnia Foundation's logs database. There are three different types of logs in Omnia: System Logs, Queue Logs and Feature Logs.

System Logs is a general-purpose place for information or error logs from both Omnia Foundation and extensions. System Logs can be viewed in Omnia admin app at **System > Logs**

.. image:: /images/omnia-admin-system-logs.png

Queue Logs contains logs from queue messages jobs. For errors related to long-running operations like uploading extension packages or creating site collection, check the Queue Logs. Queue Logs can be viewed in Omnia admin app at **System > Queues**

.. image:: /images/omnia-admin-queue-logs.png

The last type of logs is Feature Logs, which contains logs from custom code in feature activation, upgrade or deactivation. Feature Logs can be viewed in the feature detail page.

.. image:: /images/omnia-admin-feature-logs.png

Logging in extension Web API
--------------------------------------------------

In extension API that inherit from **SharePointContextProvidedController** you can use the built-in **Logging** service to write to the System Logs. 

.. code-block:: c#

    [HttpGet]
    [Route("api/documents")]
    public ApiOperationResult<IEnumerable<Document>> GetDocuments()
    {
        try
        {
           // Web API code here
        }
        catch (Exception ex)
        {
            // The built-in Logging service can be used to write to System Logs 
            this
                .Logging
                .AddLog(this.GetType().ToString(), ex.Message, DefaultLogTypes.Error, ex);

            return ApiUtils.CreateErrorResult<IEnumerable<Document>>(ex);
        }
    }

For API that does not inherit from SharePointContextProvidedController, you can use **OmniaApi** factory to create the **Logging** service.

.. code-block:: c#

    [HttpGet]
    [Route("api/documents")]
    public ApiOperationResult<IEnumerable<Document>> GetDocuments(
        string tokenKey, string spUrl, string language)
    {
        try
        {
           // Web API code here
        }
        catch (Exception ex)
        {
            // For controller that does not inherit from SharePointContextProvidedController 
            // we need to create the ClientContext first.
            ClientContext ctx = SharePointContextProvider
                                .CreateUserClientContext(tokenKey, spUrl, language);

            OmniaApi
                .WorkWith(Ctx.Omnia())
                .Logging()
                .AddLog(this.GetType().ToString(), ex.Message, DefaultLogTypes.Error, ex);

            return ApiUtils.CreateErrorResult<IEnumerable<Document>>(ex);
        }
    }

Logging in extension features
--------------------------------------------------

In Omnia feature you can write to System Logs using **OmniaApi** factory or write to Feature Logs using the built-in **Log** method 

.. code-block:: c#

    [FeatureDefinition(
       id: "85544C6C-9EB9-4F99-9410-95F1EA3D07B5",
       name: "MyOmniaExtension Sample Feature Core",
       version: "0.1.0",
       scope: FeatureScopes.Tenant
    )]
    public class SampleFeatureCore : Omnia.Foundation.Extensibility.Features.OmniaFeature
    {
        /// <summary>
        /// Activates the OmniaFeature
        /// </summary>
        public override void Activate()
        {
            // This will write to System Logs
            this.WorkWith().Logging()
                .AddLog("SampleFeatureCore", "Feature activation", DefaultLogTypes.Info);

            // This will write to Feature Logs
            this.Log("Feature activation", "Success", FeatureInstanceLogTypes.Information);
        }
    }
    
Logging in extension jobs
--------------------------------------------------

Similar to features, in jobs you can use the **OmniaApi** factory to write to System Logs. One important thing to note is that currently any uncachted error in queue job will be write to the Queue Logs, but for timer jobs you need to handle the error and explicitly write to the System Logs in your code.

.. code-block:: c#

    public void SampleJobTimer([TimerTrigger("01:00:00")] TimerInfo timerInfo)
    {
        try
        {
            // Your job code here
        }
        catch (Exception ex)
        {
            WorkWith().Logging().AddLog("SampleJobTimer", ex.Message, DefaultLogTypes.Error, ex);
        }
    }
    
    public void SampleJobQueue([QueueTrigger("SampleJob")] object queueMessage)
    {
        try
        {
            // Your job code here
        }
        catch (Exception ex)
        {
            WorkWith().Logging().AddLog("SampleJobQueue", ex.Message, DefaultLogTypes.Error, ex);
        }
    }