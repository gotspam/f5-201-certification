Lab – tcpdump Packet Capture
----------------------------

In this exercise are going to perform **tcpdump** packet captures and review the results.

Task 1 – Packet Captures of multiple interfaces simultaneously
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Open SSH session window1, and enter on one line to perform capture in background:

   - tcpdump –ni client\_vlan –eXs 0 –w /var/tmp/dump.cap & tcpdump –ni server\_vlan –eXs 0 –w /var/tmp/dump2.cap &

#. Browse to **http://10.1.10.100**

#. Enter the following commands to stop captures:

   - Type fg then <crtl> c

   - Repeat, type fg then <crtl> c

#. Enter the following command to read packet captures

   - tcpdump –r /var/tmp/dump.cap & tcpdump –r /var/tmp/dump2.cap

   .. ATTENTION::
      Q1. What is the alternate method for capturing two interfaces simultaneously?

      Q2. What interface does 0.0 represent?

      Q3. What interface typically represents the management interface?

      Q4. What is recommended method for packet captures on high load system?

      Q5. Will tcpdump capture PVA accelerated traffic?
