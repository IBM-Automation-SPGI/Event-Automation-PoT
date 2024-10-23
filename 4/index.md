Connecting real-time events to a promotional marketing app
===

### INDEX

*   [1. Introduction](../index.md)
*   [2. Creating an event stream from MQ data using Event Streams](../1/index.md)
*   [3. Building a self-service catalog of events with Event Endpoint Management](../2/index.md)
*   [4. Configuring the solution with Event Processing no-code editors](../3/index.md)
*   [5. Connecting real-time events to a promotional marketing app](./)

<br>

* * *

Connecting real-time events to a promotional marketing application
==================================================================


It's finally time to unify the hard work of Focus Corporation's integration and marketing teams. With a rich feed of filtered and processed event streams, which joins together `New Customers` and `Orders` data feeds, the marketing department can begin supplying their customer loyalty application with input.

When new events are detected which match the marketing team's promotional criteria, the application will automatically trigger an upsell activity for the new customer.

* * *

1.  Return to the authoring canvas dashboard for `StudentXXNewCustomerLargeOrder` within the **IBM Event Processing (EP)** tab.
    <br>

2.  From the list of nodes along the left-side interface, drag and drop the **Event destination** node into the canvas. Place it far to the right of the other nodes within the table . You can use your mouse wheel to resize the table if necessary.
    
      
    
    The new node `sink_1` represents a "resource", a concept from Apache Kafka. These resources can receive incoming events — thereby serving as a _destination_.
    
    [![](../images/4-2.png)](../images/4-2.png)<br><br><br>
    


3.  With your cursor, hover over the `DetectNewCustomerLargeOrders` node that was created in the previous module.
    
    *   Click and drag the node's _Output Port_ edge, connecting it to the _Input Port_ edge of the `sink_1` node
        
    *   Release the mouse button to establish the connection between the two nodes
        
    <br>

4.  Hover over the `sink_1` node and click the **Edit** (pencil) icon to configure the destination.
    
      
    [![](../images/4-3.png)](../images/4-3.png)<br><br><br>
    


5.  Within the _Details_ tab, adjust the following variables:
    
    *   **Node name** : `OutputToMarketingApp`
        
    *   **Server** : `ademo-es-kafka-bootstrap.cp4i.svc:9095`
        
    *   When ready, click **Next** 
        
    
      
    
    The server has been pre-configured as part of the hands-on lab Setup module, generating the Apache Kafka events that will simulate both "historical" and "live" customer interactions with Focus' online store and payment gateway.
    
    [![](../images/4-5.png)](../images/4-5.png)<br><br><br>
    


6.  Select the **Accept certificates** toggle when asked if you wish to trust all security certificates issued by the event stream source.
    
    When ready, click **Next** .
    
      
    [![](../images/4-6.png)](../images/4-6.png)<br><br><br>
    


7.  Within the _Access credentials_ tab, modify the following variables:
    
    *   **Username** : `es-admin`
        
    *   **Password** : the password unique to your Event Streams instance
        
    *   When ready, click **Next** 
        
    
      
    [![](../images/4-7.png)](../images/4-7.png)<br><br><br>
    


8.  Within the _Topic selection_ tab, you will be asked to specify which of the available endpoints topics should serve as a destination "sink."
    
    In this situation, the goal is to send all valid customer interactions (which meet the purchasing criteria) to the marketing department's customer loyalty application.
    
    *   Select the `STUDENTXX.LOYALTY.APP` topic - you may need to tab through the pages (3 total), or search for the Topic Name within the search bar provided at the top of the panel
        
    *   Click **Configure** to finalize the event destination
        
    
      
    [![](../images/4-8a.png)](../images/4-8a.png)
    
    [![](../images/4-8b.png)](../images/4-8b.png)<br><br><br>
    

    ---

    You are now ready to begin testing the end-to-end flow using historical event data. This will allow the marketing and integration teams to see the results of the flow and validate the results.

    If successful, several new customers who are eligible for the promotional discount should be detected and flagged within the loyalty application.

    ---

9.  Return to the authoring canvas dashboard for `StudentXXNewCustomerLargeOrder` within EP.
    
    *   In the top-right corner of the interface, click the **Run** drop-down menu
        
    *   From the list of options, select **Include historical** 
        
    
      
    [![](../images/4-9.png)](../images/4-9.png)<br><br><br>
    


10. Detected events will begin automatically populating the **Output Events** panel, along the bottom of the page.
    
    *   It will take a few moments for the events to begin populating the table
        
    *   After several hours, the table will be populated with pages of entries (cycle through these using the arrows or drop-down page selector)
        
    *   Continue to verify the detected events until satisfied with the historical event data results
        
    *   When ready, stop the flow by clicking the **Stop** button in the top-right corner of the page
        
    
    <details>
    <summary><span style="color:orange">TROUBLESHOOTING — UNABLE TO CONNECT TO RESULTS</span></summary>
    
    <p>You may experience a pop-up window in the top-right corner of the authoring canvas after initiating a Run execution. This is expected given that not all of the Kafka topic endpoints were configured — you only configured the STUDENTXX.LOYALTY.APP endpoint, as well as the STUDENTXX.ORDERS and STUDENTXX.CUSTOMERS event streams.</p>
    
    <p>You can safely ignore this warning and click the X icon to dismiss it.</p>
    
      
    <img src="../images/4-10b.png" alt="image">
    
    </details>
    <br>
    
    [![](../images/es-final-output.png)](../images/es-final-output.png)<br><br><br>
    

* * *

Conclusion
==========

Having successfully tested the flow, Focus Corporation is ready to reconfigure the event destination sink and begin feeding live event stream data into their production environment. Afterwards, it will immediately begin generating actionable promotions within Focus Corporation's customer loyalty application.


[Previous: Configuring the solution with Event Processing no-code editors](../3/index.md)  

Copyright © 2024 IBM
