elsag_import
----------------

This script parses data from the Elsag license plate database, reprocesses the images with OpenALPR, and uploads the data to an OpenALPR Web Server.

This should be installed on a server with network access to the Elsag MSSQL database and file access to the image share.  

Installation
-------------

Install Python 3.6 from here:

Download the OpenALPR script package and extract to C:\OpenALPR\elsag_import\

Download NSSM.exe https://nssm.cc/download

Add C:\openalpr\sdk to your system path

Open a command line window as administrator (Start -> Run -> cmd) and type:

    cd C:\OpenALPR\elsag_import\
    nssm.exe OpenALPRElsag
    Use your Python.exe path, and working directory for Python.exe.  For arguments, use: C:\OpenALPR\elsag_import\src\import.py

Configuration
--------------

Download the OpenALPR SDK (tested with version 2.6.103):
https://deb.openalpr.com/windows-sdk/openalpr64-sdk-2.6.103.zip

Extract the files to C:\Openalpr\sdk\
Download this file (for version 2.6.103): http://www.openalpr.com/downloads/libvehicleclassifierpy.dll

Request an evaluation license key from here:
https://license.openalpr.com/evalrequest/

and place the value in C:\Openalpr\sdk\license.conf

Edit the file:
C:\OpenALPR\elsag_import\import_config.ini

Set the following properties in the configuration to match your environment:

    database_server = IP address/hostname of the database
    database_user = Username to login (e.g., domain\user or sa)
    database_password = 
    base_image_path = The folder where the Elsag images are stored

    openalpr_url = Where to upload metadata (e.g., https://cloud.openalpr.com/push/)
    openalpr_company_id = Company ID must match your account settings on the OpenALPR Cloud Server
    openalpr_agent_uid = Arbitrary unique agent id (e.g., ELSAGIMPORTER1)

For each ELSAG camera, add a section (CameraName must match exactly what is represented in the Elsag database):

    [CameraName]
    camera_id = Unique number value
    gps_latitude = latitude of the camera
    gps_longitude = longitude of the camera


Test
-------

Test the configuration by:

    Open import_config.ini and delete the line log_file.  This will force log messages to be shown on the terminal screen
    Open a command prompt and type C:\OpenALPR\elsag_import\import.bat

Run
------

The service should now be installed as a Windows service.  Check that it is running with:

    start -> run -> services.msc
    Check that there is a service named "OpenALPR Elsag Import"
    If not started, select the service and click the Start button.
    Edit the properties and configure it to Start Automatically on Boot.

All log entries will be written to a file in C:\OpenALPR\elsag_import\log\  You can check this file to see the operation of the import when troubleshooting.