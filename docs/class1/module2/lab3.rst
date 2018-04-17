Lab – Load Balancing
--------------------

Task – Load Balancing
~~~~~~~~~~~~~~~~~~~~~

In the task, you will look and the various effects of different load balancing configurations.

#. Open the **www\_pool Members** tab.

#. Note the load balancing method on the pool and the **Ratio** and **Priority** settings on the members. Select each member and update them to the following:

   .. list-table::
      :widths: 60 60 60
      :header-rows: 1

      * - **Member**
        - **Ration**
        - **Priority**
      * - 10.1.20.11
        - 5
        - 10
      * - 10.1.20.12
        - 1
        - 10
      * - 10.1.20.11
        - 1
        - 5


#. Go to Local **Traffic > Pools > Statistics** and clear the **www\_pool** statistics.

#. Browse to **http://10.1.10.100** and refresh or **<ctrl> F5** several times.

   .. ATTENTION::
      Q1. Which **www\_pool** members was traffic sent to?

      Q2. Did member **10.1.20.11** receive the most traffic? Why not?

#. Under the **Members** tab change **Load Balancing Method** to **Ratio (member)** then **Update**.

#. Clear stats for **www\_pool** and browse **http://10.1.10.100** several times.

   .. ATTENTION::
      Q3. Which **www\_pool** members was traffic sent to?

      Q4. Did member **10.1.20.11** receive the most traffic?

Task – Priority Group Activation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Change **Priority Group Activation** to less than 2 and **Update**.

#. Clear stats for **www\_pool** and browse to **http://10.1.10.100**.

   .. ATTENTION::
      Q1. Which **www\_pool** members was traffic sent to?

#. On the pool statistics page, select member **10.1.20.11:80** and change the **State** to **Disable.**

#. Clear the statistics for the **www\_pool** and browse to http://10.1.10.100 several times.

   .. ATTENTION::
      Q2. Which **www\_pool** members was traffic sent to? Why?

      Q3. Would the results have been different if 10.1.20.11:80 had been marked offline or marked with a yellow triangle?

   .. IMPORTANT::
      Once you have complete the lab, change the **Load Balancing Method** to **Round Robin**, **Priority Group** to **Disabled**, and **Enable** pool member **10.1.20.11:80**

Task – The Effects of Persistence on Load Balancing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this task, you will enable persistence on the **www\_vs** and see the effects of persistence on load balancing. You will also see where to view persistence records that are maintain by the BIG-IP.

#. Enable a **Persistence Profile** on **www\_vs** by opening the virtual server and selecting the **Resources** tab.

#. Assign the following persistence profiles;

   .. list-table::
      :widths: 80 60
      :header-rows: 0

      * - **Default Persistence Profile**
        - cookie
      * - **Fallback Persistance\_Profile**
        - source\_addr


   *If you see an error requiring an HTTP profile, go to Properties and assign the default HTTP profile.*

   .. ATTENTION::
      Q1. Why was a http profile required?

#. Clear stats for **www\_pool** and browse to **http://10.1.10.100**.

   .. ATTENTION::
      Q2. Was traffic evenly distributed to all **www\_pool** members? Why not?

#. In the web page under **HTTP Request and Response Information** is **Display Cookie** link.

   - Select **Display Cookie** to view the cookie created by the BIG-IP.

   - Open **Statistic > Module Statistics > Local Traffic > Persistence Records**.

   - Click on pool member displayed on persistence record and **Disable** the pool member.

   - Browse to **http://10.1.10.100**.

   .. ATTENTION::
     Q3. Did you persist to the Disabled member? Why?

#. Change status of persisted pool member to **Forced Offline**.

#. Note: Persisted Records still exist. Browse to http://10.1.10.100.

   .. ATTENTION::
      Q4. Does traffic continue to persist to the member Forced Offline?

      Q5. If cookies were disable on your browser would persistence still work? Why?

   Alternate method to display persistence is: **tmsh show ltm persistence persist-records**.
