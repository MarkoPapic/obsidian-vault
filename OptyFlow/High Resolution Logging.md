
Section **2.9 High Resolution Data Manual Download from Controllers** of https://fdotwww.blob.core.windows.net/sitefinity/docs/default-source/research/reports/fdot-bdv27-977-06-rpt.pdf explains how to download High Resolution logs for multiple controllers, including Econolite and Maxtime.

## Econolite

Econolite controllers have a file for each 15 minutes of logs.
So we fetch this file via FTP every 15 minutes.

| SNMP Object ID                  | Mnemonic                        | Data Type | Value                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------- | ------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1.3.6.1.4.1.1206.3.5.2.9.17.1.0 | asc3DataLogEnable               | Boolean   | 0 = Logging Not Enabled<br>1 = Enable Logging<br>2 = Disable Logging<br>3 = Enable Logging with ftp upload<br>_(User must program server IP address MM1-5-1 and have ftp server running. Data is uploaded to the root directory of the ftp server. File is deleted after successful ftp. User must also manually ftp up and delete older files)_ |
| 1.3.6.1.4.1.1206.3.5.2.9.17.2.0 | asc3DataLogCircularBufferEnable | Boolean   | 1 = Enable Circular Logging. Oldest files are deleted to make room for new File. ftp delete does not work in this mode<br>2 = Disable Circular Logging. No more files can be created after Flash is full. ftp delete works in this mode.\|                                                                                                       |
   
On a new ASC/3 controller, the default value of _asc3DataLogEnable_ is **OFF** and the default value of _asc3DataLogCircularBufferEnable_ is **ON**. Upon power-up the controller will not be logging any data.
To log data asc3DataLogEnable **Set** to **1** using the ASC3 SNMP Client (shown to right).
To disable circular buffer data _asc3DataLogCircularBufferEnable_ must be **Set** to 2 using the ASC3 SNMP Client (shown to right).
To get the current setting, press on the **Get** button on the Utility.
You can download the data files via FTP. If circular buffering is enabled you don’t need to delete the old data files that you have downloaded. This is will be taken care of automatically. If circular buffering is disabled then you will need to use FTP to delete data files after you download them.
Place the asc3Aries.ini file in the same folder as the utility program. When you start it up it will display the above OIDs as the first top 2 items. Do **Set** or **Get** operations as described above.
Default ASC/3 ftp _username/password_ is **econolite/ecpi2ecpi**. User selected username/ password can be achieved upon request.

