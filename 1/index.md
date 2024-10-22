
Creating an event stream from MQ data using Event Streams
===

### INDEX

*   [1. Introduction](../index.md)
*   [2. Creating an event stream from MQ data using Event Streams](./)
*   [3. Building a self-service catalog of events with Event Endpoint Management](../2/index.md)
*   [4. Configuring the solution with Event Processing no-code editors](../3/index.md)
*   [5. Connecting real-time events to a promotional marketing app](../4/index.md)

<br>

* * * 

Creating an event stream from an IBM MQ message queue
===

To take advantage of the message queue data available from IBM MQ, Focus Corporation's integration team will need to make IBM MQ's enterprise data available to a broader set of APIs. This will allow Focus' application developers to subscribe to the data without any risk or impact to the core back-end systems that support the data. Risks will be lowered and the application development process can be decoupled from data retention processes.

To do so, Focus' integration team will need to expose the IBM MQ enterprise data using "event streams." Specifically, the integration team (and application developers) will need access to the customer order information contained within these streams. This data will be vital for the marketing team's plans to offer high value promotions for newly-acquired customers in a timely manner.

<br>

* * *

Configuring IBM MQ to clone customer order data
===============================================

Both the order management system and its payment gateway system are currently using IBM MQ message queues for exchanging customer order data. The integration team will need to tap into this message exchange, clone each of the orders being handled across IBM MQ, and then publish those messages into a new event stream.

1.  Open the **IBM MQ** dashboard with a web browser.
    
    From the left-hand side interface, drill down into the **Manage** tab.
    
      
    [![](../images/1-1.png)](../images/1-1.png)<br><br><br>

2.  Select the **Queues** tab along the top of the page.
    
      
    [![](../images/1-2.png)](../images/1-2.png)<br><br><br>


3.  Scroll down the page until you reach a table, which at this time should show several queues that are already a part of the environment.
    

    Create a new queue by clicking the blue **Create** icon in the top-right corner of the table.
    
      
    [![](../images/1-3.png)](../images/1-3.png)<br><br><br>


4.  Wait for the Queue creation wizard to load, then select **Local** from _Choose queue type_.
    
      
    [![](../images/1-4.png)](../images/1-4.png) <br><br><br>
    

5.  The **Quick create** option should be selected by default.
    
    *   Under the **Queue name** field, enter `STUDENTXX.TO.KAFKA` (all uppercase)
    *   Leave all other settings configured to their default values
    *   When ready, click **Create**
    *   The web browser will refresh back to the **Manage** > **Queues** perspective
    *   From the Queues table, confirm that the `STUDENTXX.TO.KAFKA` queue is now available
    
      
    [![](../images/1-5.png)](../images/1-5.png)
    

    Next, you need to configure IBM MQ to clone all customer order data that it is exchanging between Focus Corporation's order management system and payment gateway. Messages must be reviewed to verify that they contain the expected payload information.  

    <br><br><br>


6.  From the **Manage** > **Queues** table, locate the queue named `STUDENTXX.PAYMENT.REQ` and click the **name** to inspect it in more detail.
    
      
    [![](../images/1-6.png)](../images/1-6.png) <br><br><br>
    


7.  The queue will already have been populated with multiple _Messages_. Click on any of the **Timestamp** names available in the table (something similar to `Feb 21, 2024 at 5:12:00 PM`) to pull open a _Message Details_ panel.
    
    *   Scroll down the _Message Details_ panel until you reach **Application Data**
    *   Inspect the contents of the packet, which is a series of key-value pairs: you should see details about the order `id`, `customer`, `customerid`, `description`, `price`, `quantity`, `region`, and `ordertime`
    *   **Close** the _Message Details_ panel by clicking the `X` in the top-right corner or the grey `Close` button
    
      
    [![](../images/1-7.png)](../images/1-7.png) <br><br><br>
    


8.  Back on the `STUDENTXX.PAYMENT.REQ` orders summary table, locate the **Actions** button in the top-right corner of the page. Click to open a drop-down menu and then select **View configuration**.
    
      
    [![](../images/1-8.png)](../images/1-8.png) <br><br><br>
    


9.  Multiple attributes of the `STUDENTXX.PAYMENT.REQ` order queue can be configured from this page.
    
    Click the grey **Edit** button to the right side of the _General_ page.
    
      
    [![](../images/1-9.png)](../images/1-9.png) <br><br><br>
    


10. From the tabs on the left side of the page, drill down into **Storage**.
    
    *  Scroll down until you reach the **Streaming queue name** field and change the value to `STUDENTXX.TO.KAFKA`.  
    *  This will direct IBM MQ to clone messages from the `STUDENTXX.PAYMENT.REQ` queue into the `STUDENTXX.TO.KAFKA` streaming queue created in _Step 5_.  
    *  When satisfied, click the blue **Save** button in the top-right of the page to confirm the configuration changes.  
    
      
    [![](../images/1-10.png)](../images/1-10.png) <br><br><br>
    


11. Once confirmed, the new configuration will immediately take effect and cloned order messages will promptly begin filling the `STUDENTXX.TO.KAFKA` queue.
    
    *  Scroll back up to the top of the page and locate the blue **Manage** text in the top-left corner of the screen  
    *  Click the text to return back to the _Manage_ page for `Orders`  
    *  From the tabs along the top of the page, click the **Queues** tab  
    *  From the table of queues, drill down into the `STUDENTXX.TO.KAFKA` queue  
    
      
    [![](../images/1-11a.png)](../images/1-11a.png) <br><br><br>
    


12. Take note of the customer order messages that are now populating the `STUDENTXX.TO.KAFKA` message queue table. You can click the circular refresh icon (to the left of the blue _Create_ button) to reload the queue contents.

    <br>

    ---

    ### Cloning order queues with IBM Event Streams

    ---

    Focus Corporation's integration team will now need to create an event stream called `Orders` using _IBM Event Streams_. This will serve as the repository where messages, cloned from IBM MQ, are published and made available to other parts of the organization.

    The integration team will need to make decisions about how to replicate the data (with or without modification) and also determine the appropriate retention settings for this data. Given the governance policies in place at Focus Corporation, they will need to retain data for up to 1 week and replicate entries for high availability.

    <details>
    <summary><span style="color:lightblue">MQ-KAFKA CONNECTOR</span></summary>  

    <p>IBM MQ allows applications, systems, services, and files to request and coordinate processing tasks — sending and receiving message data via messaging queues. **IBM Event Automation**'s Kafka integrations makes it possible to capture a continuous stream of events, representing state changes across one or multiple environments, and makes those events persistently available for retrieval.</p>

    <p>In combination, IBM MQ and Event Automation enable business-critical communications originating over MQ to be captured as events within Kafka topics, which can later be shared across the enterprise for fueling more responsive applications. IBM MQ-Kafka connectors support bi-directional connectivity between these two architectures. Clients of IBM Event Automation are fully supported for using MQ-Kafka connectors with the Event Streams capability of the platform. Support for the MQ-Kafka connector is also available for IBM clients with MQ Advanced entitlements. </p>
    </details>  

    <br>
    



13. Open the **IBM Event Streams** tab using your web browser.<br>


14. From the _IBM Event Streams_ dashboard, click the **Create a topic** tile.
    
      
    [![](../images/1-13.png)](../images/1-13.png)<br><br><br>
    

15. The team must first decide on a **Topic Name**.
    
    Set the value to `STUDENTXX.ORDERS` and then click the blue **Next** button to continue.
    
      
    [![](../images/1-15.png)](../images/1-15.png)<br><br><br>
    

16. Under the **Patitions** tab, accept the default recommendation of `1` by clicking **Next**.
    
      
    [![](../images/1-16.png)](../images/1-16.png)<br><br><br>
    

17. Under the **Message Retention** tab, accept the default recommendation of `A week` and click **Next** to continue.
    
      
    [![](../images/1-17.png)](../images/1-17.png)<br><br><br>
    

18. Under the **Replicas** tab, accept the default recommendation of `Replication factor: 3` and confirm your selections by clicking the **Create Topic** button.
    
      
    [![](../images/1-18a.png)](../images/1-18a.png)<br><br><br>
    

    ---

    ### Configuring a message bridge between IBM MQ and IBM Event Streams

    Using the Apache Kafka connector framework, Focus Corporation's integration team will now need to configure an "event bridge" using Red Hat OpenShift. The task can be performed programmatically via the OpenShift console. The bridge configuration will include connectivity details for accessing both IBM MQ and IBM Event Streams.

    Once configured and deployed, the bridge will utilize the Apache Kafka connector framework to read messages from the `STUDENTXX.TO.KAFKA` _message queue_ and then publish those to the newly-created `STUDENTXX.ORDERS` _event stream_.

    ---
    <br>

19. Return to the OpenShift container platform dashboard.
    
    From the **Home** page, click the **+** icon located in the top-right corner of the interface.
    
      
    [![](../images/1-19.png)](../images/1-19.png)<br><br><br>
    

20. The interface will load an **Import YAML** configuration tool, with a black canvas awaiting input. Here you can supply _YAML_ (Yet Another Markup Language) or _JSON_ files to define new deployments on the OpenShift cluster.
    
    The YAML definition of the Apache Kafka connector "bridge" has been prepared ahead of time. Copy and paste the following YAML _exactly_ as written into the **Import YAML** canvas (remember to change XX to your student ID):
    

    ```yaml
    apiVersion: eventstreams.ibm.com/v1beta2
    kind: KafkaConnector
    metadata:
      name: studentxx-mq-connector
      namespace: cp4i
      labels:
        eventstreams.ibm.com/cluster: kafka-connect-cluster
    spec:
      class: com.ibm.eventstreams.connect.mqsource.MQSourceConnector
      tasksMax: 1
      config:
        # the Kafka topic to produce to
        topic: STUDENTXX.ORDERS
        # the MQ queue to get messages from
        mq.queue: STUDENTXX.TO.KAFKA
        # connection details for the queue manager
        mq.queue.manager: orders
        mq.connection.name.list: orders-ibm-mq(1414)
        mq.channel.name: SYSTEM.DEF.SVRCONN
        # format of the messages to transfer
        mq.message.body.jms: true
        mq.record.builder: com.ibm.eventstreams.connect.mqsource.builders.JsonRecordBuilder
        key.converter: org.apache.kafka.connect.storage.StringConverter
        value.converter: org.apache.kafka.connect.json.JsonConverter
        # whether to send the schema with the messages
        key.converter.schemas.enable: false
        value.converter.schemas.enable: false
    ```
    <br>

  
    When ready, click **Create**. Full deployment should only take a moment.

  
    [![](../images/1-20a.png)](../images/1-20a.png)<br><br><br>


21. Switch over to the **IBM Event Streams** tab with your web browser. Having configured the streaming queue in IBM MQ (earlier in Step 18), Focus Corporation's integration team now wants to view the orders that have been generated so far.
    
    From the home dashboard of the IBM Event Streams service, click the **Topic** tab (left-hand side) and then click on the name `STUDENTXX.ORDERS` from the table.
    
      
    [![](../images/1-21a.png)](../images/1-21a.png)<br><br><br>
    


22. Granular details about the `STUDENTXX.ORDERS` topic will be loaded within the browser. From this page, you can inspect all of the messages (orders) generated from the time you set up the IBM MQ streaming queue configuration earlier.
    
    *   Click any one of the orders to pull up additional details on the payload and its contents
    *   Note that you can inspect details about the `quantity`, `price`, `customerid`, `description`, `id`, `region`, `ordertime`, and `customer`
    
      
    [![](../images/1-22.png)](../images/1-22.png)<br><br><br>
    
  
These fields will be valuable later for the marketing team as they look to perform outreach on customers meeting certain criteria.

* * *

Next Steps
==========

In the following module, the integration team will use _IBM Event Endpoint Management_ to import the event stream data and curate a self-service catalog for the marketing department.

[Previous: Introduction](../index.md)  

[Next: Building a self-service catalog of events with Event Endpoint Management](../2/index.md)

Copyright © 2024 IBM