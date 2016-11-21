Scheduled Job
============================

.. code-block:: c#

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


Scheduled jobs will run at the interval defined in the **TimerTrigger**. There are 2 different contructors you can use:    

.. note:: The minimum interval supported by Omnia Jobs is 10 seconds

.. note:: **TimerInfo** is a place-holder for future features, currently it contains no information

.. code-block:: c#
    
    public void MyJobTimer([TimerTrigger("01:00:00")] TimerInfo timerInfo)
    
.. code-block:: c#
    
    public void MyJobTimer([TimerTrigger(1, 0, 0)] TimerInfo timerInfo)
   