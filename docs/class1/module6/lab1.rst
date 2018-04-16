Lab – Modify and Troubleshoot Virtual Servers
---------------------------------------------

Task – Troubleshooting virtual servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By now, I am sure you are dying to know what’s up with the **purple\_vs**. Here’s a chance to find out. You are going to some troubleshooting with a little guidance.

#. Go to **Network Maps** and take a look at the status of the **purple\_vs** and its components.

   *It is obvious that all pool members are offline which could be anything, a network issue, a server issue, a BIG-IP configuration issue.*

   .. ATTENTION::
      Q1. Where would you start?

#. SSH to **bigip01** at 10.1.1.245.

   .. ATTENTION::
      Q2. Attempt to ping he pool members. Does it work? What does this tell you?

      Q3. Attempt a **curl -i** against the pool members. Does it work? What does this tell you?

      Q4. Since the problem affects all pool members, what would you suspect as a possible issue?

#. Find the issue with the pool members and correct the issue.

   .. HINT::
      You may want to read https://support.f5.com/csp/article/K2167

   .. ATTENTION::
      Q5. Did you correct the issue? (If not go to **Appendix 1 – Answer Key** and see how the issue was fixed)

      Q6. Now the pool is working and purple\_vs is available can you access the page through the virtual?

      Q7. What is your next step in debugging? Is the virtual server processing traffic?

#. You need to watch traffic from your PC to the BIG-IP virtual server and from the BIG-IP to the pool.

   .. ATTENTION::
      Q8. What command(s) could you use to watch traffic hit the virtual server and leave toward the pool?

   *(Try to figure it out, if you need help go to Appendix 1 – Answer Key and my version of the commands)*

   .. ATTENTION::
      Q9. Did you see traffic hit the virtual server? Did you see BIG-IP send traffic to a pool member?

      Q10. Did you see the return traffic? If there was no response, what is your step?

#. The server’s default gateway is 10.1.20.240, which is an **unused** IP address on the 10.1.20.0/24 network. There were two ways to resolve the virtual server issue. Your **purple\_vs** should now be **available**.

   *(If you need help go to **Appendix 1 – Answer Key** and my version of the commands)*

Task – Working with profiles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Create new virtual server **secure\_vs 10.1.10.100:443** with TCP profile, Automap and **www\_pool**.

#. Browse to **https://10.1.10.100** and observe tcpdump.

   .. ATTENTION::
      Q1. Did site work? Why not?

#. Change SSL Profile to include clientssl then update

#. Browse to **https://10.1.10.100** and observe tcpdump

   .. ATTENTION::
      Q2. Did site work?

#. Enable cookies Default Persistence Profile and update? Note error and troubleshoot to fix.

   .. ATTENTION::
      Q3. What was needed to add cookie persistence?

#. Browse to **https://10.1.10.100/index.php** and select Display Cookie on bottom of page.

   .. ATTENTION::
      Q4. What is the name of the cookie inserted begin with?

#. Create new pool **secure\_pool** with members of **10.1.20.11:443, 10.1.20.12:443** and **10.1.20.13:443** and assign to **sure\_vs**.

#. Browse to **https://10.1.10.100**

   .. ATTENTION::
      Q5. Did site work?

#. Troubleshoot and fix.

   .. ATTENTION::
      Q6. What profile was needed to correct the error?
