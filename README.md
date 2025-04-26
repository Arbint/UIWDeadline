# Deadline Documentation

Qickstart:

## Install Deadline Client
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

