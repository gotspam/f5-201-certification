Module 1 - Packet Processing and Virtual Servers
================================================

Lab Preparation and Packet Processing
-------------------------------------

Open BIG-IP TMSH and TCPDump session
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Why are ssh sessions not displayed in connection table?*

**tmsh show sys connections** displays connections on the TMOS data plane.
SSH connections are established to out-of-band management interface and
thus not seen.

Establish ftp connection
~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. In the tcpdump above, what is client IP address and port and the
server IP address port?*

10.1.10.1:60603 and 10.1.10.20:21 (FTP)

.. NOTE::

   60603 is an ephemeral port and BIG-IP will attempt to use the same
   client port on the server-side connection

*Q2. What is source ip and port as seen by ftp server in the example
above?*

Source IP: 10.1.20.249 Source IP: 61236

*Q3. What happened to the original client IP address and where did
10.1.20.249 come from?*

The virtual server was configured to do source address translation using
the SNAT Pool, SNAT249\_pool. Reviewing the configuration of
SNAT249\_pool shows it was configured with IP address 10.1.20.249.

Packet Filters
--------------

Test the FTP packet filter
~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Was the existing ftp connection in the connection table affected?
Why?*

The FTP connection is not affected because adding packet filter does not
impact established connections.

*Q2. Was ftp connection successful? If yes, why?*

The attempt to establish a new FTP connection was blocked, because the
packet filter rule applies to all new connection attempts

*Q3. What did tcpdump reveal? Connection timeout or reset?*

Tcpdump revealed multiple **S** (syn) attempts without receiving ack. This
is indicating a connection timeout.

*Q4. What did virtual server statistics for ftp20\_vs reveal? Why are
counters not incrementing?*

VS stats shows no new connection attempts because Filter is applied
before VS in order of processing

*Q5. Prioritize the packet processing order:*

Virtual Server **3** SNAT **4** AFM/Pkt Filter **2** NAT **5** Existing
Connections **1** Self IP **6** Drop **7**

Virtual Server Packet Processing
--------------------------------

Testing Virtual Server Packet Processing Behavior
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Which VS is used for web traffic over port 8080?*

wildcard\_vs

*Q2. Which VS is used for ftp traffic?*

ftp\_vs

*Q3. Which VS is used for web traffic over the default HTTP port? Which
port was used?*

www\_vs port 80

*Q4. Which VS is used for web traffic?*

wildcard\_vs

Module 2 - Virtual Server and Pool Behavior and Status
======================================================

Virtual Server Status
---------------------

Test Disabled Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. What is the Availability of* **www\_vs**\ *? What is the State?*

Availability: available, State: disabled

*Q2. What symbol is used to represent* **www\_vs** *status?*

Black Circle

*Q3. Would you expect browsing to http://10.1.10.100 to work?*

No

*Q4. Can you ping the virtual IP?*

Yes, the virtual address still responds to pings

*Q5. Did the site work? What did the tcpdump show?*

No, the tcpdump showed the virtual server 10.1.10.100:80 responding to
SYNs with Resets

*Q6. Did statistics counters for any virtual increment?*

No

*Q7. Why do you think the* **wildcard\_vs** *didn't pick up the packets?*

www\_vs was the most specific virtual server so it responded. Because the www_vs was disabled the response was to reset the connection.  This make sense if you think about it.  What good would it do to disable a virtual server just to have another virtual server pick up the traffic either process incorrectly or send it to servers you just tried to prevent traffic from going too.

*Q8. What symbol is used to represent* **wildcard\_vs**\ *? Why is symbol a
square?*

The status symbol is a black square. Black because the virtual server
was administratively disabled and square because there is no monitor and
the state is Unknown

*Q9. What is the reason given for current state?*

The children pool member(s) either don't have service checking enabled,
or service check results are not available yet. Availability: unknown
State: disabled

*Q10. Does ftp session still work? Why?*

Disabling a configuration item (node, pool or virtual server) does not
affect existing connections.

*Q11. Did new ftp session establish connection? Why not?*

No, a disabled virtual server will not process new connections.

Virtual Server Connection Limits and Status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Does ftp session work?*

Yes

*Q2. What is the virtual server status of ftp\_vs?*

Yellow Triangle - Availability: unavailable - State: enabled

*Q3. Did new ftp session establish connection? Why not?*

No, the virtual server's connection limit has been reached.

*Q4. Did tcpdump capture show a connection reset?*

Yes, tcpdump revealed **R** TCP reset the connection.

Pool Member and Virtual Servers
-------------------------------

Effects of Monitors on Members, Pools and Virtual Servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Since the* **mysql\_monitor** *will fail, how long will it take to
mark the pool offline?*

60 seconds, the monitor will have to fail 4 times at 15 second intervals
before it exceeds the 46 second timeout value.

*Q2. What is the icon and status of* **www\_vs**\ *?*

Red Diamond - Availability: offline - State: enabled - The children pool
member(s) are down

*Q3. What is the icon and status of* **www\_pool**\ *?*

Red Diamond - Availability: offline - State: enabled - The children pool
member(s) are down

*Q4. What is the icon and status of the* **www\_pool** *members?*

Red Diamond - Availability: offline - State: enabled - Pool member has
been marked down by a monitor

*Q5. Does pool configuration have an effect on virtual server status?*

Yes, the status of the pool members can affect the status of the virtual
server.

*Q6. What is the icon and status of* **www\_vs**\ *?*

Black Diamond - Availability: offline - State: disabled - The children
pool member(s) are down

*Q7. Did traffic counters increment for* **www\_vs**\ *?*

No

*Q8. What is the difference in the tcpdumps between Offline (Disabled) vs
Offline (Enabled)?*

Offline (Disabled) - immediate connection reset, you will see no virtual
server statistics.

Offline (Enabled) - initial connection accepted then reset, the virtual server stats are
incremented

More on status and member specific monitors
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. What is the status of the Pool Member and the monitors assigned to
it?*

Red Diamond - Red Diamond - Availability: offline - State: enabled -
Pool member has been marked down by a monitor

http - Green Circle, mysql\_monitor - Red Diamond

*Q2. What is the status of* **www\_vs**, **www\_pool** *and the pool
members? Why?*

Green, Green, Red, Red, Green. One pool member available, marks the pool
available and since the pool is available, the virtual server is
available

*Q3. Did the site work?*

Yes

*Q4. Which* **www\_pool** *members was traffic sent to?*

Traffic was distributed to availble pool members.

Load Balancing
--------------

Load Balancing
~~~~~~~~~~~~~~

*Q1. Which* **www\_pool** *members was traffic sent to?*

Traffic was distributed to 10.1.20.12 and 10.1.20.13

*Q2. Did member 10.1.20.12 receive the most traffic? Why not?*

No, because LB method is Round Robin, Ratio and Priority Group
configurations on pool members do not apply.

*Q3. Which* **www\_pool** *members was traffic sent to?*

Traffic was distributed to 10.1.20.12 and 10.1.20.13

*Q4. Did member 10.1.20.12 receive the most traffic?*

10.1.20.12 received 5x more traffic than 10.1.20.12

Priority Group Activation
~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Which* **www\_pool** *members was traffic sent* to?

Traffic was distributed to 10.1.20.11 and 10.1.20.12

*Q2. Which* **www\_pool** *members was traffic sent to? Why?*

Traffic was distributed to 10.1.20.12 and 10.1.20.13. Pool member
availability dropped below 2 available members in the highest priority
group and the next lowest priority group was activated.

*Q3. Would the results have been different if 10.1.20.11:80 had been
marked offline or marked with a yellow triangle?*

No, both mark the member as Unavailable, dropping the Available members
below 2.

The Effects of Persistence on Load Balancing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Why was a http profile required?*

The http profile was required to tell the BIG-IP to parse the http
request/response sequence for the virtual server so it could insert and
read cookies in the http headers.

*Q2. Was traffic evenly distributed to all* **www\_pool** *members? Why
not?*

Traffic went to only on pool member because of persistence,

*Q3. Did you persist to the Disabled member? Why?*

Yes, a Disable pool member will still receive new connections if a
persistence record points to it.

*Q4. Does traffic continue to persist to the member Forced Offline?*

No, another available member was selected and a new persistence record
was created

*Q5. If cookies were disable on your browser would persistence still
work? Why?*

Yes, source address persistence would be used to persist to a pool
member

Module 3 - Trouble-shooting the BIG-IP
======================================

Trouble-shooting Hardware
-------------------------
End User Diagnostics
~~~~~~~~~~~~~~~~~~~~

*Q1. What three methods are available for running EUD on F5 Hardware?*

USB CDROM, USB Bootable Drive, Hardware Boot Menu

*Q2. How do you determine EUD version?*

EUD image downloaded or eud\_info

*Q3. What is the filename and location of the EUD output?*

/shared/log/eud.log

LCD Panel
~~~~~~~~~

*Q1. How do you halt the unit via the LCD panel?*

Press X, select system menu, press check, select halt, press check to
confirm

*Q2. Holding the X for 4 seconds does what?*

Powers down unit

*Q3. Holding the Check button for 4 seconds does what?*

Reboots the unit

Hardware Log Files
~~~~~~~~~~~~~~~~~~

*Q1. What is the filename and location of the logs for LTM?*

/var/log/ltm

*Q2. Where will power supply, fan and hard disk related issues be logged?*

/var/log/ltm

HA and Failover
~~~~~~~~~~~~~~~

*Q1. Is failover sometimes used to determine issues related to hardware or software?*

hardware

*Q2. How do you initiate failover to standby unit?*

From Active unit select Network > Traffic Groups, select traffic group, select Force to Standby

*Q3. What persistence profile cannot be mirrored?*

Cookie persistence is not mirrored

*Q4. What two connections types are re-mirrored after failback?*

Only FastL4 and SNAT connections are re- mirrored after failback

*Q5. When would you recommend using connection mirroring?*

Long lived connections

*Q6. Where is connection mirroring configured?*

You can configure connection mirroring at VS and SNAT

*Q7. Where is persistence mirroring configured?*

You can configure persistence mirroring at Persistence

*Q8. What tmsh command is used to view mirrored connections?*

show /ltm persistence persist-records

*Q9. What tmsh command is used to view mirrored persistence?.*

show /ltm persistence persist-records

*Q10. What can be the cause of primary unit returning to active state after initiating failover to standby?*

Show /sys connection all-properties

tcpdump Packet Capture
----------------------

Packet Captures of multiple interfaces simultaneously
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. What is the alternate method for capturing two interfaces
simultaneously?*

tcpdump -ni eth1 -w /var/tmp/dump1.cap **&** tcpdump -ni eth2 -w
/var/tmp/dump2.cap

*Q2. What interface does 0.0 represent?*

All interfaces

*Q3. What interface typically represents the management interface?*

eth0

*Q4. What is recommended method for packet captures on high load system?*

F5 recommends that you mirror traffic to a dedicated sniffing device

*Q5. Will tcpdump capture PVA accelerated traffic?*

No, you must disable PVA to capture traffic

Performance Statistics
----------------------

Observing performance statistics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. What is the longest time interval available for performance
statistics?*

30 Days

Connectivity Troubleshooting
----------------------------

Connectivity troubleshooting tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Was echo response received?*

Ping reply was successful

*Q2. What is the status of the virtual servers?*

ftp\_vs and www\_vs available, disabled - wildcard\_vs unknown, disabled

*Q3. Was echo response received?*

Ping reply successful

Self IP Port Lockdown
---------------------

Effects of Port Lockdown
~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Was echo response received?*

Ping reply successful

*Q2. Was ssh successful? Why not?*

No. Port lockdown set to **Allow None** by default

*Q3. Was ssh successful?*

Yes

*Q4. Does existing ssh window still work?*

No

*Q5. Was new ssh session established?*

No

Module 4 - Support and Analytics
================================

Support, Status and Logs
------------------------

Qkview and iHealth
~~~~~~~~~~~~~~~~~~

*Q1. Are logs associated with qkview?*

Yes

*Q2. Where is default filename and location of qkview output?*

/var/tmp/hostname.qkview

*Q3. Where is the default filename and location of core dump?*

/var/core/

*Q4. What is Severity and Condition for unit failure in active/standby
pair?*

Severity 2, Site at Risk

*Q5. If support case was opened online with Severity 4 and no call has
been received in a week. What should you do?*

Call support, reference open case and ask to escalate. This may require
Duty Manager approval.

*Q6. What is the procedure to escalate support case?*

Call support, reference open case and ask to escalate. This may require
Duty Manager approval.

Network Map
~~~~~~~~~~~

*Q1. What is a node?*

IP Address of Pool Member

*Q2. What icon is reflected for 10.1.20.11 on the Network map?*

Black

*Q3. What is the color of the icon for pool members based on 10.1.20.11?  Why?*

Grey Circle

*Q4. Does ftp\_vs still work as expected?*

No

*Q5. Where is irule reflected on Network Map?*

iRule is displayed between the Virtual Server and Pool

Dashboard
~~~~~~~~~

*Q1. What is longest duration available for reporting?*

1 Month

*Q2. How can report be exported?*

Reports may be exported as csv files.

Log files
~~~~~~~~~

*Q1. Was an alert logged?*

Yes

*Q2. Was the alert logged here?*

Yes

*Q3. What command is needed to find all instances of err in /var/log/ltm?*

grep err /var/log/ltm

iApps and Analytics
-------------------

Create iApps Analytics
~~~~~~~~~~~~~~~~~~~~~~

*Q1. Did both pool members respond? Why?*

No, only one responded because cookie persistence was built using the
iApp

*Q2. Can you determine which page took the longest to load?*

If you select Latency > Page Load Time from the top bar you will find
/bigtext.html took longest.

*O3. Could you add the pool member? Why?*

No, because iApp strictness is on by default and the application can
only be changed by going to the iApp application and selecting
Reconfigure from the top bar

*Q4. Can you add the custom_analytics profile to the ftp_vs? Why?*

No, analytics in v11.5 can only be done on HTTP and DNS virtual servers
with a HTTP or DNS profile attached.

Module 5 - Managing the BIG-IP
==============================


UCS, BIG-IP Archive
-------------------

Create UCS Archive Files
~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. What extension must Archive have?*

.ucs

*Q2. What is the default location for ucs files?*

/var/local/ucs

*Q3. What is command for loading ucs file?*

load /sys ucs <path to UCS>

load /sys ucs <path to UCS> no-license - This will not restore the license
file

*Q4. What issues will occur by restoring ucs file on RMA device?*

Licensing and device cert keys must be updated.

Upgrading a BIG-IP Device Service Clusters (DSC)
------------------------------------------------

Upgrading software
~~~~~~~~~~~~~~~~~~

*Q1. You are about to start your upgrade to 12.1, which device will you
upgrade first?*

You would begin the upgrade on the standby device, in this case that
should be bigip02.

*Q2. True or false? Once the install is complete, the BIG-IP will
automatically reboot to the new volume.*

False, you will have to set the new volume as the Active volume and then
reboot the BIG-IP

*Q3. What steps would be required to complete the upgrade?*

1. Set the new volume to the active volume

2. Reboot the BIG-IP

3. Confirm the reboot was successful and the BIG-IP is running

4. Force the BIG-IP with the old software to Standby, making virtual
   servers, and other listeners active on the upgraded BIG-IP

5. Test that traffic is passing correctly.

6. Upgrade the BIG-IP with the older software.

BIG-IQ
------

Peruse BIG-IQ
~~~~~~~~~~~~~

*Q1. What BIG-IPs are being managed?*

bigip01.f5demo.com and bigip02.f5demo.com

*Q2. Where are configurations currently being display from?*

The configuration displayed was retrieved from the BIG-IP

*Q3. What is the difference between displaying from BIG-IQ and displaying
from BIG-IP?*

If you are displaying configuration from BIG-IP the configuration is
maintained and updated on that BIG-IP. If you are displaying
configuration from the BIG-IQ, then BIG-IQ owns the configuration and
can push changes out to the BIG-IP, no change should be made to the
BIG-IP directly.

*Q4. What now appears in the Nodes title when you hover the mouse over
it?*

A (**+**) appears in the title area because you can now modify the device
via the BIG\_IQ.

Make a modification via the BIG-IQ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Was new\_node added to bigip02?*

No, it was not.

*Q2. What is being added? What is in the New Version window.?*

new\_node is being added and the REST commands to do that are show in
the New Version window.

*Q3. Check bigip02, was new\_node created?*

Yes

Module 6 - Modify and Troubleshoot Pools and Virtual Servers
============================================================

Modify and Troubleshoot Virtual Servers
---------------------------------------

Troubleshooting virtual servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*Q1. Where would you start?*

I would go on the BIG-IP and test connectivity from the BIG-IP to the
pool members.

*Q2. Attempt to ping one of the pool members. Does it work? What does
this tell you?*

The ping should be successful. This means the server IP is up and I have
basic connectivity.

*Q3. Attempt a* **curl -i** *against a pool member. Does it work? What does
this tell you?*

The curl should be successful and you should see the request come back.
The application is running.

*Q4. Since the problem affects all pool members, what would you suspect
as a possible issue?*

Since I can see all pool members are functioning I would suspect the
monitor is the issue. You could start debugging the monitor directly, or
you could put the default HTTP monitor and see if the pool members
come up. If they do, then the monitor is the issue and needs correction.
In the case, you would check the Send and Receive strings. I would use a
curl -i (to include the header and response codes) to look for the
receive string. In this case it's obvious, we are looking for a 200 OK
(successful reponse), but have fat-finger 020 OK in the Receive box.
Correct the receive string and reapply the monitor. The pool should come
up Available (Green).

.. Note::

   The default HTTP monitor usually, but does not always, work on an HTTP application.

*Q5. Did you correct the issue?*

Yes

*Q6. Now the pool is working and purple\_vs is available can you access
the page through the virtual?*

No

*Q7. What do you think would be the next step in debugging the issue
would be?*

I would clear the virtual server statistics and try again and see if the
traffic is hitting purple\_vs. The virtual server statistics should show
traffic being processed.

*Q8. What command(s) could you use to watch traffic hit the virtual
server and leave toward the pool?*

I would create two tcpdumps one on the client-side and the other on the
server-side. I would want to limit the captures to watch for my PC IP
address 10.1.10.51. You will need two terminal windows.

Terminal Window 1 (Client to BIG-IP)

**tcpdump -i client\_vlan -X -s0 host 10.1.10.51 and 10.1.10.105**

(This command will only watch client-side traffic between the PC and
virtual server. The -s0 command will dump the entire packet -X command
will dump hex and ascii code of the packet. You will be able to see the
HTTP request and response in the dump)

Terminal Window 2 (BIG-IP to Pool)

**tcpdump -i server\_vlan -X -s0 host 10.1.10.51**

(This command will only watch server-side traffic from the PC and to the
pool. The -s0 command will dump the entire packet -X command will dump
hex and ascii code of the packet. You will be able to see the HTTP
request and response in the dump)

*Q9. Did you see traffic hit the virtual server? Did you see BIG-IP send
traffic to a pool member?*

You should have seen traffic hit the virtual server in Window 1 and in
Window 2 BIG-IP should have picked a pool member and sent traffic to it.

*Q10. Did you see the return traffic? If there was no response, what is
your step?*

No, you should not have received a response. Because the BIG-IP is not
the default gateway, so the response went someplace else.

1. You can add and SNAT Pool or do SNAT Automap on the virtual server.

2. You can add 10.1.20.240 as a self IP address on the BIG-IP. This
   should be a floating IP in traffic\_group\_1 so that the default
   gateway for the servers is still available upon failover.

Working with profiles
~~~~~~~~~~~~~~~~~~~~~

*Q1. Did site work? Why not?*

SSL connection error

*Q2. Did site work?*

Yes

*Q3. What was needed to add cookie persistence?*

http profile

*Q4. What is the name of the cookie inserted begin with?*

BIGipServerwww\_pool

*Q5. Did site work?*

No

*Q6. What profile was needed to correct the error?*

Server side ssl profile
