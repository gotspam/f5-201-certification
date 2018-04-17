Lab – Virtual Server Packet Processing
--------------------------------------

Task 1 – Create additional Virtual Servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a wildcard virtual server and pool, test and observe various
traffic under different configurations to determine how virtual servers
process new inbound connections. You will be using tcpdump from window1,
virtual server statistics, as well as a browser to determine behavior.

#. Create **wildcard\_vs** **10.1.10.100:\*** with TCP profile, Automap and **wildcard\_pool** of **10.1.20.11:\***

   - To create the wildcard pool, go to **Local Traffic > Pools > Pool List** and select **Create**.



   .. list-table::
      :widths: 60 60
      :header-rows: 0

      * - **Name**
        - wildcard\_pool
      * - **Address**
        - 10.1.20.11
      * - **Port**
        - \*

   *Don’t forget to Add the pool member to the New Members box before you hit Finished.*

#. To create the wildcard virtual server, go to **Local Traffic > Virtual Server** and select **Create**.

   .. list-table::
      :widths: 60 60
      :header-rows: 0

      * - **Name**
        - wildcard\_vs
      * - **Destination**
        - 10.1.10.100
      * - **Service Port**
        - \*
      * - **Source Address Translation**
        - Automap
      * - *Default Pool**
        - wildcard\_pool

   *Don’t forget to hit Finished.*

   .. NOTE::
      You didn’t enter the source addresses allowed. Go to your new virtual server and look at the **Source** to see the default source addresses allowed.


Task – Testing Virtual Server Packet Processing Behavior
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Many of your virtual servers have the same virtual address. You will now
test various behaviors.

#. Clear virtual server stats.

#. Observe connection statistics (VS stats) for each of the following

   -  Browse to **http://10.1.10.100:8080**

   .. ATTENTION::
      Q1. Which VS is used for web traffic over port 8080?

   -  FTP to 10.1.10.100

   .. ATTENTION::
      Q2. Which VS is used for FTP traffic?

   -  Browse to http://10.1.10.100

   .. ATTENTION::
      Q3. Which VS is used for this web traffic the default HTTP port? What port was used?

#. Clear virtual server stats.

#. Modify the **wildcard\_vs** to only allow connections from a **Source** of 10.1.10.0/24

#. Browse to **http://10.1.10.100**

   -  Observe connection statistics (VS stats)

   .. ATTENTION::
      Q4. Which VS is used for web traffic?

#. Clear virtual server stats.

#. Modify **wildcard\_vs** to include the default **Source** of 0.0.0.0/0.
