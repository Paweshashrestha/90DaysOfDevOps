```
# Day 09 Challenge

## Users & Groups Created

Users:
- tokyo
- berlin
- professor
- nairobi

Groups:
- developers
- admins
- project-team


## Group Assignments

developers:
- tokyo
- berlin

admins:
- berlin
- professor

project-team:
- tokyo
- nairobi


## Directories Created

/opt/dev-project
permissions: 775
group: developers

/opt/team-workspace
permissions: 775
group: project-team


## Commands Used

sudo useradd -m tokyo
sudo useradd -m berlin
sudo useradd -m professor
sudo passwd tokyo
sudo passwd berlin
sudo passwd professor

sudo groupadd developers
sudo groupadd admins

sudo usermod -aG developers tokyo
sudo usermod -aG developers berlin
sudo usermod -aG admins berlin
sudo usermod -aG admins professor

sudo mkdir /opt/dev-project
sudo chgrp developers /opt/dev-project
sudo chmod 775 /opt/dev-project

sudo useradd -m nairobi
sudo groupadd project-team
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo

sudo mkdir /opt/team-workspace
sudo chgrp project-team /opt/team-workspace
sudo chmod 775 /opt/team-workspace


## What I Learned

1. Linux manages users through /etc/passwd and groups through /etc/group
2. Groups allow multiple users to share permissions on directories
3. chmod and chgrp control access and collaboration in Linux systems
```
