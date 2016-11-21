Omnia Jobs
============================


Omnia Jobs are pieces of code for handling long-running operations that can be run either as a message queue or as a scheduled timer job.   

Create new Omnia Job
--------------------------------------------------

You can create Omnia Job using the template from Omnia Tooling

.. image:: /images/toolings-item-templates-jobs.png

.. code-block:: c#

    [JobDefinition(
       id: "368D7722-9F75-4789-A1BA-460DBB6595F8",
       name: "MyJob",
       description: ""
    )]
    public class MyJob : OmniaJob
    {
        // Scheduled Job job function
        public void MyJobTimer([TimerTrigger("01:00:00")] TimerInfo timerInfo)
        {
            try
            {
                // Your job code here
            }
            catch (Exception ex)
            {
                WorkWith().Logging().AddLog("MyJobTimer", ex.Message, DefaultLogTypes.Error, ex);
            }
        }

        // Message Queue job function
        public void MyJobQueue([QueueTrigger("MyJob")] object queueMessage)
        {
            try
            {
                // Your job code here
            }
            catch (Exception ex)
            {
                WorkWith().Logging().AddLog("MyJobQueue", ex.Message, DefaultLogTypes.Error, ex);
            }
        }
    }

The job metadata is defined by the attribute **JobDefinition**. Each Omnia Job can contains any number of **independent** job functions, each can be of one of the two types Scheduled Job or Message Queue.

Built-in methods and properties
--------------------------------------------------

Similar to Omnia Feature and Omnia Web API, Omnia common services are available through the built-in **WorkWith** method. The code in Omnia Jobs is run at tenant scope so there is no user context, you can get an app-only ClientContext using the method **CreateContextFor**

+-------------------+---------------+---------------------------------------------------------------+
| Properties        | Type          | Description                                                   |
+===================+===============+===============================================================+
| Tenant            | Tenant        | | The current tenant.                                         |
|                   |               |                                                               |
+-------------------+---------------+---------------------------------------------------------------+
| OmniaInstanceMode | Enum          | | The mode of the current tenant.                             |
|                   |               | | could be Tenant or Sitecollection                           |
|                   |               |                                                               |
+-------------------+---------------+---------------------------------------------------------------+

+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+
| Methods                                                            | Type          | Description                                                   |
+====================================================================+===============+===============================================================+
| CreateContextFor(string spUrl)                                     | ClientContext | | Create an app-only context                                  |
|                                                                    |               |                                                               |
+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+
| WorkWith()                                                         | ApiFactory    | | Return the ApiFactory that can call Omnia API               |
|                                                                    |               | | Example: WorkWith().Logging().AddLog(log)                   |
+--------------------------------------------------------------------+---------------+---------------------------------------------------------------+


Manage jobs in a tenant
--------------------------------------------------

Unlike tenant resources, jobs are available and running immediately after the extension is deployed, there is no need to activate any feature.

All jobs deployed by extensions can be viewed in admin app at **Systems > Jobs**. You can also change the interval of scheduled jobs or stop/force run them from this interface. Note that built-in jobs will not be displayed here.

.. image:: /images/omnia-admin-system-jobs.png

Job Types
------

.. toctree::
    :titlesonly:

    scheduled
    queues    