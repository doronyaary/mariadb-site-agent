# MSA MariaDB-Site-Agent
MSA is a tool that was designed to turn off the service of MariaDB once other servers are not reachable, and by that enabling site preference on a Galera implementation.

# Installation
The installation process is pretty simple. Just download the tar.gz file of the latest release and run:

```bash
tar xf msa-2024.01.tar.gz
./install.sh
```

# Usage
In order to run MSA, you would need to create an INI file that holds all of the configurations. This is an example (pretty much all of it) of a typical INI file - see the table at the end for explanations.

```ini
[msa]
logfile=msa.log
peers=10.0.0.1:3306,10.0.0.2:3306
mandatorypeers=10.0.0.1:3306
user=msa
password=MyUserPassword?
servicestop=systemctl stop mariadb
servicestart=systemctl restart mariadb
servicestatus=systemctl is-active --quiet mariadb
interval=20
```

This is the information for each and every configuration:

| Configuration | Explanation |
| - | - |
| logfile | Optional. Sets the log file that MSA will write information/warnings/errors to. |
| peers | Sets the list of the replication peers of the cluster in a comma separated list while each server is written as SERVER:PORT |
| mandatorypeers | Sets the minimum available peers that **must** be available for the local service to be running |
| user | Sets the MariaDB user to connect to each and every peer |
| password | The user's password |
| servicestop | The systemctl command to stop the service |
| servicestart | The systemctl command to restart the service |
| servicestatus | The systemctl status command that returns only the error code - if you are not sure, leave is as its shown above here |
| interval | The interval in seconds for each heartbeat check |

# Running
The following command will run MSA:

```bash
./msa --conf=msa.ini
```

TIP: If can wrap this command in a unit file (service).
