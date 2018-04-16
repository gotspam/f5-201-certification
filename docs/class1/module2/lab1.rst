Lab – Virtual Sever Status
--------------------------

Task – Test Disabled Virtual Servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this task, you will disable and enable various virtual servers and note the behavior.

#. Disable **www\_vs** from the **Virtual Server List** or from within the **www\_vs** GUI interface.

#. Open **Local** **Traffic > Virtual Servers** and hover over status icons.

#. From window2 (TMSH) type: **show ltm virtual or show ltm virtual www\_vs**

   .. ATTENTION::
      Q1. What is the Availability of **www\_vs**? What is the State?

      Q2. What symbol is used to represent **www\_vs** status?

      Q3. Would you expect browsing to **http://10.1.10.100** to work?

      Q4. Can you ping the virtual IP?

#. Clear virtual server stats and browse to **http://10.1.10.100**

#. Observe the tcpdump (window1) and connection statistics in the Virtual Server statics GUI interface.

   .. ATTENTION::
      Q5. Did the site work? What did the tcpdump show?

      Q6. Did statistics counters for any virtual increment?

      Q7. Why do you think the **wildcard\_vs** didn’t pick up the packets?

#. Disable **wildcard\_vs** and note the State and Availability of the virtual servers.

   .. ATTENTION::
      Q8. What symbol is used to represent **wildcard\_vs**? Why is symbol a square?

      Q9. What is the Reason given for current state?

#. Establish ftp connection to 10.1.10.100 and ensure successful login.

#. Disable **ftp\_vs**.

   .. ATTENTION::
      Q10. Does ftp session still work? Why?

#. Open another window and establish ftp connection to 10.1.10.100.

   .. ATTENTION::
      Q11. Did new ftp session establish connection? Why not?

.. IMPORTANT::
   Make sure all virtual servers are Enabled before continuing.*

Task – Virtual Server Connection Limits and Status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this task, you will set the connection limit for the FTP virtual server to 1 and note the status and behavior of different connection scenarios.

#. Modify **ftp\_vs** for connection limit of 1. The **Connection Limit** option can be found under the **Advanced** virtual server menus.

#. Establish ftp connection to **10.1.10.100** and hold the logon open.

   .. ATTENTION::
      Q1. Does FTP session work?

      Q2. What is the virtual server symbol and status of **ftp\_vs**?

#. Open another window and establish a second ftp connection to 10.1.10.100.

   .. ATTENTION::
      Q3. Did new ftp session establish connection? Why not?

      Q4. Did tcpdump capture connection reset?

      Q5. Quit all FTP sessions and note **ftp\_vs** status.
