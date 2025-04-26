# Deadline Setup On Ubuntu

Deadline works with 5 different component

* ```Deadline Clients```
    
    The appliations that users install on their end to interact with deadline. (Deadline Monitor, Deadliine Worker...)

* ```MongoDB```

    The data base Deadline uses to store job status, priority, dependencies, work infomations, pools, groups, limits, user settings, and statistics


* ```Deadline Repository```

    A repository(folder) Deadline uses to store plugins, scripts, render logs, configure files. it should not be used for maya projects.

* ```Deadline Remote Conection Server```

    A server running on the server machine that:
    - Talks to the deadline repository and MongoDB
    - All deadline clients connects to it to access the deadline repository and MongoDB, it acts like the middle man between clients and MongoDB and Dedline Repository.

* ```SMB or NFS shared network location```

    A share network location that all render workers can access, this is the place a maya project is placed, before rendering.


# Setup Steps:

- The order of installation/setup is:

    1, MongoDB

    2, Deadline Repository
     
    3, Deadline Remote Connection Server

    4, SMB file server 

    5, Deadline Client on user machines

## Step 1 - MongoDB

[Reference Steps](https://www.cherryservers.com/blog/install-mongodb-ubuntu-2404)

upgrade the system:
```
sudo apt update
```

```
sudo apt upgrade
```
install curl (a downloading tool):

```
sudo apt install -y gnupg curl
```

Use the cURL utility to import the MongoDB pbulic GPG key:

```
curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor
```

Add the MongoDB repository to the list of sources on your system:
```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
```
Update system again:
```
sudo apt update
```

Install MongoDB
```
sudo apt install -y mongodb-org
```

Start and enable MongoDB

```
sudo systemctl start mongod
sudo systemctl enable mongod
```

Check MogoDB status
```
sudo systemctl status mongod
```
If the status log shows failed or exited:
* be sure to double check the version being installed is compatiable with the operating system and cpu. easiest way to do that is search for ```install MongoDB on Ubuntu 24.04``` (search for how to install on your specific OS and version)

* be sure MongoDB has permissions on

    /var/log/mongodb

    /var/lib/mongodb

Try mongo shell
```
mongosh
```
### MongoDB configs:
use this command to open the config file:
```
sudo vim /etc/mongod.conf
```

Change the MongoDB data base path by setting dbPath:
```
# Where and how to store data.
storage:
    dbPath: /your/prefered/database/path
```


Allow MongoDB to listen on any incomming connections by chaning the bindIp to ```0.0.0.0```

```
# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0
```
After config, restart MongoDB
```
sudo systemctl restart mongod
```
without any doubt, your mongod stops working, because MongoDB do not have access to the new data base location you specified.

We should give mongodb ownership of the following:

```
sudo chown -R mongodb:mongodb /your/prefered/database/path
sudo chown -R mongodb:mongodb /var/log/mongodb
sudo chown -R mongodb:mongodb /var/lib/mongodb
```
Try restart MongoDB again, and it should start to work again.
```
sudo systemctl restart mongod
```
# Step 2 - Install Deadline Repository

```
NOTE:
When installing deadline repository
Deadline Repository does not need to be given any certificate
The certificate setting during install is ONLY used to
authenticate with MongoDB, and our MongoDB was not set to require one.

The certificate needed by the client will be generated
when setting up the Remote Connection Server (Installed Through the 
Client Installer) 

When Installing the Remote Connection Server, It will also ask for 
a certificate to communicate with MongoDB, we can ignore that one 
for the same reason. After that step, the installer will promote 
to generate certificate for the clients
```

First, we will need to [download](https://docs.thinkboxsoftware.com/products/deadline/10.4/1_User%20Manual/manual/download-deadline.html) it through AWS, you will need an aws account to do so.

After downloading the linux installer, you can scp it to the server.
```
scp Deadline-<version>-linux-installers.tar user@serverip:/home/user/Downloads/
```


