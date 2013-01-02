AzureTraceListeners
========================

Configuration
-------------

See below for specific examples.

ApplicationName can be any value you wish to group trace logs on.

ConnectionString should be:

a storage client string for Tables and Queues ie:

    DefaultEndpointsProtocol=https;AccountName=ACCOUNTNAME;AccountKey=ACCOUNTKEY or useDevelopmentStorage=true
	
an ADO.NET connection string for Azure SQL ie:
  
    Data Source=.\SQLEXPRESS;Initial Catalog=LogDatabase;Integrated Security=True

Table/Queue name defaults to TraceLogs - it can be customised as per the usual rules for Azure Table and Queue names. 

Setting up the AzureTableTraceListener
--------------------------------------

    Trace.Listeners.Add(new AzureTableTraceListener(applicationName, connectionString, tableName));

Setting up the AzureQueueTraceListener
--------------------------------------

    Trace.Listeners.Add(new AzureQueueTraceListener(applicationName, connectionString, queueName));
	
Setting up the AzureSqlTraceListener
------------------------------------

    CREATE TABLE [dbo].[TraceLogs] (
        [Id] [int] IDENTITY(1,1) NOT NULL,
        [ApplicationName] [nvarchar](max) NOT NULL,
        [Category] [nvarchar](max) NULL,
        [Timestamp] [datetime2](7) NOT NULL,
        [Message] [nvarchar](max) NOT NULL,
        CONSTRAINT [PK_TraceLogs] PRIMARY KEY CLUSTERED 
        (
            [Id] ASC
        )
    )
    
    Trace.Listeners.Add(new AzureSqlTraceListener(applicationName, connectionString, tableName));
    
Roadmap
-------

* Improved documentation
* Splitting the listeners such that if you only need the Azure SQL listener, you don't need to include the table storage reference?
* As part of the above, remove the Azure SQL / Queue listeners' use of TableServiceEntity
* Add Transient Fault Handling to the Azure SQL listener
* Add retry logic if a trace message cannot be logged?
* Ability to customise/extend the fields to be logged