Lab – Support, Status and Logs
------------------------------

Task – Qkview and iHealth
~~~~~~~~~~~~~~~~~~~~~~~~~

#. Open System->Support page.

#. Ensure QKView is selected then click Start.

#. Download snapshot file and upload ihealth.f5.com (login is required).

   .. ATTENTION::
      Q1. Are logs associated with qkview?

#. From ssh window run qkview

   .. ATTENTION::
      Q2. Where is default filename and location of qkview output?

      Q3. Where is the default filename and location of core dump?

      Q4. What is Severity and Condition for unit failure in active/standby pair?

      Q5. If support case was opened online with Severity 4 and no call has been received in a week. What should you do?

      Q6. What is the procedure to escalate support case?

Task – Network Map
~~~~~~~~~~~~~~~~~~~~

#. Explain status icons of objects on network map.

   - Open **Local Traffic > Network Map** and hover over icons and observe status info.

   - Ensure all icons are green. If not, then troubleshoot.

   *Note the top-down status relationship between VS, pools, pool members and nodes.*

   .. ATTENTION::
      Q1. What is a node?

#. Open **Local Traffic > Nodes** and disable node 10.1.20.11.

   .. ATTENTION::
      Q2. What icon is reflected for 10.1.20.11 on the Network map?

      Q3. What is the color of the icons? Why?

      Q4. Does **ftp\_vs** still work as expected?

#. Attach irule to virtual server via Network Map.

   - Select **www\_vs** from Network Map.

   - Select Resources > Manage irules.

   - Enable \_sys\_https\_redirect irule and click Finished.

   .. ATTENTION::
      Q4. Where is irule reflected on Network Map?

Task - Dashboard
~~~~~~~~~~~~~~~~~~

#. Observe Dashboard statistics

   - Log on to the BIG-IP GUI using Firefox and go to **Statistics >> Dashboard**

   .. ATTENTION::
      Q1. What is longest duration available for reporting?

      Q2. How can report be exported?

Task 4 – Log files
~~~~~~~~~~~~~~~~~~

#. Interpret the LTM log file

   - Open ssh window1 and enter the following command:

     - tail –f /var/log/ltm

#. Disable **ftp\_vs**

   .. ATTENTION::
      Q1. Was alert logged?

#. Go to **System > Logs > Local Traffic**

   .. ATTENTION::
      Q2. Was the alert logged here?

#. From ssh window 1 enter **<CTRL> C** and at the CLI prompt enter:

   - **grep alert /var/log/ltm**

   - **grep www\_pool /var/log/ltm**

   .. ATTENTION::
      Q3. What command is needed to find all instances of err in /var/log/ltm

   - **grep err /var/log/ltm**
