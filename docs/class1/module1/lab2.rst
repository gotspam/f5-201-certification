Lab – Packet Filters
--------------------

Task – Create a packet filter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You are going to test how packet filters impact packet processing by creating a packet filter to block ftp connections to 10.1.10.100.
Follow these steps to complete this task:

#. Go to **Network > Packet Filters > Rules** and Create a filter using the following:

   .. list-table::
      :widths: 60 60
      :header-rows: 0

      * - **Name**
        - Block_ftp
      * - **Order**
        - First
      * - **Action**
        - Discard
      * - **Destination Hosts**
        - 10.1.10.100
      * - **Destination Port**
        - 21 (FTP)
      * - **Logging**
        - Enabled

*Make sure you select Add after enter a host/network or a port.*

Task – Test the FTP Packet Filter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ensure ftp connection is established. (See 2.1.1.3. Task – Establish ftp connection)

#. Go to **Network > Packet Filters > General** and select **Enable** and then **Update**.

   .. ATTENTION::
      Q1.  Was the existing ftp connection in the connection table affected?  Why?

#. Quit ftp and clear virtual server statistics by going to Local **Traffic > Virtual Servers > Statistic**, select the virtual server and hit **Reset**.

#. Attempt to establish an ftp connection to 10.1.10.100. Note tcpdump capture in Window1.

   .. ATTENTION::
      Q2.  Was ftp connection successful?		Why?

      Q3.  What did tcpdump reveal?			Did the connection timeout or reset?

      Q4.  What did virtual server statistics for ftp_vs reveal?	Why are counters not incrementing?

      Q5.  Prioritize the packet processing order below from 1-7:

      **Virtual Server___   SNAT___   AFM/Pkt Filter___   NAT___   Existing Connections___   Self IP___   Drop ___**

#. Review the Packet Filter Logs and Packet Filter Statistics, then disable the Packet Filters.

   - Go to **Network > Packet Filters > Statistics** and review the information.

   - Go to **System > Logs > Packet Filters** and review the information.

   - Go to **Network > Packet Filters > General** and select **Disable** and then **Update**.
