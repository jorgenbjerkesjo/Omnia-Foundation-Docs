Message Queues
============================

.. code-block:: c#
    
    public void MyJobQueue([QueueTrigger("MyQueue")] object queueMessage)
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

Message queue jobs will be triggered everytime a message is added to the queue that the jobs listen to ("MyQueue" in this example). 

Queue messages can be added when a feature is activated/deactivated, when an web api endpoint is called or from another Omnia Job. To add a queue message, use the Queues service:    

.. code-block:: c#
    
    public void MyJobTimer([TimerTrigger(1, 0, 0)] TimerInfo timerInfo)
        {            
            try
            {
                // Add a queue message to the queue "MyQueue" every hour, triggering the queue message job.
                WorkWith().Queues().AddQueueMessage("MyQueue", new MyModel());
            }
            catch (Exception ex)
            {
                WorkWith().Logging().AddLog("MyJobTimer", ex.Message, DefaultLogTypes.Error, ex);                
            }
        }

        // The content of the queue message will de deserialized to type MyModel
        public void MyJobQueue([QueueTrigger("MyQueue")] MyModel queueMessage)
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

Dequeue Mode
--------------------------------------------------

When adding a queue message you can set the **transaction ID** for the message, so that multiple messages can be grouped together in the same transaction. By default, the message queue job will be triggered immediately when a new message is added, however you can change this behavior by setting the dequeue mode to **SynchronousTransaction**. 

When running synchronous dequeue mode, all messages in the same transaction will be processed sequentially by the order that they was added. In other words, if a new message of the same transaction is added while the previous message was being processed, the new message will not be processed until the previous message has been finished.

.. code-block:: c#
    
    public void MyJobQueue([QueueTrigger("MyJob", DequeueMode = Models.Queues.DequeueModes.SynchronousTransaction)] MyModel queueMessage)            