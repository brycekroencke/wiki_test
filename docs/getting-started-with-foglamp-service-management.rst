.. role::  raw-html(raw)
    :format: html
.. _foglamp-service-management:
***********************************************
Getting Started With FogLAMP Service Management
***********************************************

1. Install foglamp-service-management via (a) Package or (b) Repository
    (a) Package
        .. code-block:: console

            sudo apt install -y foglamp-service-management

    (b) Repository
        .. code-block:: console

            curl -sX POST http://localhost:8081/foglamp/service?action=install -d '{"format":"repository", "name": "foglamp-service-management"}'

        .. code-block:: JSON

            {
            	"message": "foglamp-service-management is successfully installed",
                "link": "log/200721-12-25-18-foglamp-service-management-install.log"
            }


2. Verify management service is installed
    .. code-block:: console

        curl -sX GET http://localhost:8081/foglamp/service/installed | jq

    .. code-block:: JSON

        {
          	"services": [
            	"notification",
            	"storage",
            	"south",
            	"management"
          	]
        }


3. Add management service
    .. code-block:: console

        curl-sX POST http://localhost:8081/foglamp/service -d '{"name": "FogMan Agent", "type": "management", "enabled": "true"}' | jq

    .. code-block:: JSON

        {
          "id": "0e43def5-92f3-42de-b8cc-6f91f08c9cd7",
          "name": "FogMan Agent"
        }


4. Using FogMAN GUI add the FogLAMP and configure it to Poll
    FogMAN GUI :raw-html:`&rarr;` FogLAMPs :raw-html:`&rarr;` add FogLAMP
    Set **Name** to "FogLAMP-that-polls", **Address** to the FogLAMPs IP, and the credentials to the appropriate values. **Check "FogLAMP Polls FogMAN"** then click add. If you navigate back to the FogLAMPs tab you should see a tag "Poll" next to the FogLAMP.

5. Using FogLAMP GUI configure FogLAMP for Poll
    1. FogLAMP GUI :raw-html:`&rarr;` Configuration :raw-html:`&rarr;` FogLAMP Service
    Change **Name** to FogLAMP-that-polls.
    2. FogLAMP GUI :raw-html:`&rarr;` Configuration :raw-html:`&rarr;` select FogMan Agent from dropdown
    Set **FogMAN Host** to the IP of FogMAN and configure the credentials.

6. Verify the existence of both the **FM Agent** and the **FogMAN Agent Poll** schedules
    .. code-block:: console

        curl -sX GET http://localhost:8081/foglamp/schedule | jq

    .. code-block:: JSON

        {
            "id": "ecc64ce3-6f3c-4cfb-b310-21113f04cb18",
            "name": "FogMan Agent",
            "processName": "management",
            "type": "STARTUP",
            "repeat": 0,
            "time": 0,
            "day": null,
            "exclusive": true,
            "enabled": true
        },
        {
            "id": "eb8372e8-a80b-4f3b-97ea-dca1ed19bc68",
            "name": "FogMan Agent Poll",
            "processName": "manage",
            "type": "TIMED",
            "repeat": 86400.0,
            "time": 60,
            "day": null,
            "exclusive": true,
            "enabled": true
        }


7. Update **FogMAN Agent Poll** schedule to desired poll conditions
    FogLAMP GUI :raw-html:`&rarr;` Schedules :raw-html:`&rarr;` FogMAN Agent Poll
    Change **Type** to INTERVAL and configure **Repeat (Interval)** to occur once every 5 min (``0`` ``00:05:00``) and save.

8. Verify the poll was successful
    Once the 5 minute interval has triggered the poll, new configurations will be read into the FogLAMP.
