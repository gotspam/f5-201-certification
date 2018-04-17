Lab – Upgrading a BIG-IP Device Service Clusters (DSC)
------------------------------------------------------

Task – Upgrading software
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Prior to any upgrade, you would want to backup your device and then
synchronize your changes. In the upper left corner, you should see
**Changes Pending** due to the changes you have made to
**bigip01.f5demo.com**.

#. Click on **Changes Pending** or go to **Device Management >> Overview**
and select **bigip01.**

#. The **Sync Device to Group** button should already be selected. Hit the
**Sync** button at the bottom.

#. Sometime sync get slightly off, if your sync fails select Overwrite Configuration and try again

   .. ATTENTION::
      Q1. You are about to start your upgrade to 12.1, which device will you upgrade first?

#. On the appropriate device go to **System >> Software Management**

#. Select the v12.1.2 image and hit **Install.**

#. In the **Volume set name** selection enter **upgrade**.

   *You could also have picked a volume, but for the lab you are creating a new one.*

   .. ATTENTION::
      Q2. True or false? Once the install is complete, the BIG-IP will automatically reboot to the new volume.

      Q3. What steps would be required to complete the upgrade?
