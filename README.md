# Deadline Documentation

Userful Topics:

[Administration](./Documents/Admin.md)

## Qickstart

### Install Deadline Client
First, Add the shared network location to the local machine:

Fire up a powershell, type in:
```
net use S: \\10.40.14.25\RenderSourceRepository /user:perforce uiw3d /persistent:yes
```
This will create an S drive on the computer that can be used as unified network location for all the render workers to find maya project, textures, and other assets.

```You only have to do this once per machine```


Go to the new ```S``` drive, and copy these 2 files to any folder on your machine:

```
S:/DeadlineClientInstallers/Deadline-10.4.1.6-windows-installer/DeadlineClient-10.4.1.6-windows-installer.exe
```

```
S:/DeadlineClientInstallers/Deadline10RemoteClient.pfx
```

open ```DeadlineClient-10.4.1.6-windows-installer.exe``` to install the client.

Click next on the welcome page:

<img src="./Assets/InstallClient_01_Start.png">

Accept the Agreement:

<img src="./Assets/InstallClient_02_Agree.png">

Using the Default Install location if you do not care to put it anywhere else:

<img src="./Assets/InstallClient_03_InstallLocatioin.png">

Set the Install Type to Client:

<img src="./Assets/InstallClient_04_Select_Client.png">

Set the Remote Cnnection Type to ```Remote Connection Server(Recommended)```

<img src="./Assets/InstallClient_05_UseRemoteServerConnection.png">

Both the Perforce Server and the Deadline Server is on the same server:
| Ip |Port   |
|---|---|
|10.40.14.25|4433|

<img src="./Assets/InstallClient_06_ServerAddress.png">

For the RCS TLS Certificate, use the one you copied from the ```S``` Drive. And the Certificate Password is ```uiw3d```

<img src="./Assets/InstallClient_07_Certificate.png" width=1280>

For the Deadline Launcher Setup, set it to ```Block Remote Control``` 

<img src="./Assets/InstallClient_08_BlockRemoteControl.png">

Also ```Block Auto Upgrade```

<img src="./Assets/InstallClient_10_NoAutoUpgrade.png">

Click ```Next``` in the Ready to Install step:

<img src="./Assets/InstallClient_11_ReadyToGo.png">

The installer should start install the client, should take a few minutes to finish.

Once the installer is finished, you can launch the ```Deadline Monitor``` and it should automatically connect to the deadline server:

<img src="./Assets/openMonitory01_Monitor.png">

If the conenct was not successful, you can hit ok to the error message, and it should allow you to reconfigure again, the image shows the correct settings (the Passphrase is uiw3d), and the Advanced TLS Options should be the first one.

<img src="./Assets/openMonitory02_ConnectionSettings.png">