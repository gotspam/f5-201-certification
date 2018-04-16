Lab – Self IP Port Lockdown and more
------------------------------------

Task 1 – Effects of Port Lockdown
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Ping 10.1.10.245

   .. ATTENTION::
      Q1. Was echo response received?

#. SSH to 10.1.10.245

   .. ATTENTION::
      Q2. Was ssh successful? Why not?

#. Open **Network > Self IPs > 10.1.10.245** and change **Port Lockdown** to **Allow Defaults**

#. SSH to 10.1.10.245

#. Browse to https://10.1.10.245

   .. ATTENTION::
      Q1. Did SSH work? Did browsing work?

      Q2. What other ports are opened when you select Allow Defaults.

#. Open **Network > Self IPs > 10.1.10.245** and change Port Lockdown to **Allow Custom** and add Port 22

#. SSH to 10.1.10.245

#. Browse to https://10.1.10.245

   .. ATTENTION::
      Q3. Did SSH work? Did browsing work?

Task 2 – Effects of Port Lockdown
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Open **System >> Platform**

#. On **SSH IP Allow** > **Specify Range** of 10.1.1.10-20

   .. ATTENTION::
      Q4. Does existing SSH window still work?

#. Open new SSH session to 10.1.1.245

   .. ATTENTION::
      Q5. Was new ssh session established?

Task 3 – Check DNS and NTP are configured properly
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Verify the DNS and NTP configuration and test DNS.

   - Go to **System >> Configuration >> Device >> General** and review the DNS and NTP setting

#. In BIG-IP command line terminal window (window 2) test DNS from the CLI or TMSH enter:

   - dig pool.ntp.org
