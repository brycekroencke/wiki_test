.. role::  raw-html(raw)
    :format: html
**************************************************
Getting Started With Management Poll Notifications
**************************************************

1. Set up FogLAMP Service Management
    See :ref:`foglamp-service-management`
    Before continuing this document you should have a FogLAMP configured to poll from FogMAN on a scheduled interval.

2. Install foglamp-notify-management
    .. code-block:: console

        sudo apt install -y foglamp-notify-management

3. Add notification management template
    FogMAN GUI :raw-html:`&rarr;` Templates :raw-html:`&rarr;` Add Template
    These instructions will be using this `template <https://github.com/dianomic/fogman-wiki/blob/main/wiki-ref-code/json-templates/threshold-management.json>`_, which uses the **Threshold** plugin as the rule for triggering the notification and uses the **foglamp-notify-management** plugin as the delivery method. You may add a template with any rule; however, make sure to use **foglamp-notify-management** as the delivery method.
4. Add asset to trigger notification
    FogMAN GUI :raw-html:`&rarr;` Machines :raw-html:`&rarr;` Add Machine
    Set **Machine Type** to "Signal Generator" and set **Name** to "signal generator". Now create a sinusoid connection from this machine to the FogLAMP. Set **Name** to "sinusoid", **FogLAMP** to "FogLAMP-that-polls", and leave **Asset Name** as "\$Name\$".

5. Add Event Processor to FogLAMP
    FogMAN GUI :raw-html:`&rarr;` FogLAMPs :raw-html:`&rarr;` :raw-html:`&#8942;` to the right of "FogLAMP-that-polls" :raw-html:`&rarr;` Event Processors :raw-html:`&rarr;` Add
    Set **Template** to "threshold-management" and configure the rest of the notification to match the image below.

    .. image:: images/fm-agent-setup/configure-notification.png
               :scale: 50

6. Change FogMan Agent Poll Schedule type to MANUAL
    FogMAN GUI :raw-html:`&rarr;` FogLAMPs :raw-html:`&rarr;` "FogLAMP-that-polls" :raw-html:`&rarr;` Schedules :raw-html:`&rarr;` FogMan Agent Poll
    Change **Type** to MANUAL and save.

    **NOTE:** The schedule type will change once the configuration is deployed and then polled by the FogLAMP. Now future polls will be triggered by the new notification rather than a timed interval.

7. Deploy and confirm configuration has been polled by FogLAMPs scheduled poll
    Using the FogMAN GUI, Deploy the configuration. Once the poll schedule triggers you should see three changes in the FogLAMP GUI

        1. The sinusoid
        2. The new notification
        3. The changed schedule for FogMan Poll Agent
    Now the FogLAMP is configured to poll whenever the sinusoid data point exceeds .9, with a 60 cooldown between retriggers.
