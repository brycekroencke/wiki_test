**************************************************
Setting up Alternative FogLAMP Users
**************************************************

Creating a new FogLAMP user and certificate
-------------------------------------------

    #. Ensure that you are working within an unlocked version

       * create a new version if necessary
    #. Click ⋮ next to the FogLAMP that you would like to add a new user role for
    #. Select **FogLAMP Users** then Select **Add** in the top right
    #. Provide the desired **Name**, **Username**, and **Role** then click **Create**
    #. A new certificate has been generated for that user, select and copy it
    #. Deploy the current version

Logging into the FogLAMP with the new cert
------------------------------------------

    1. Open a new browser tab and enter the IP of your FogLAMP to be directed to the FogLAMP GUI
    2. If you are brought to a login page, skip to subsection B

    A) Setting up access via https

        1. Under connection set up select **https** from the dropdown menu, change the port to 1995, then click set the URL and restart
        2. Open a new browser tab and ping the FogLAMP by entering the command below in the URL bar, replacing {FogLAMP-ip} with the IP of the FogLAMP

        .. code-block:: console

            https://{FogLAMP-ip}:1995/foglamp/ping

        3. Allow the web browser to be loaded

           * This step will differ depending on OS and Browser
        4. You will know you have succeeded once you see a webpage containing a single JSON

    B) Completing login with new cert

        1. Return to your FogLAMP browser tab and refresh, you should now see a login screen
        2. Select "Login with Certificate" below the username and password field
        3. Next select "Manually put the certificate content"
        4. Paste the certificate content from FogLAMP Management into the field and click login
