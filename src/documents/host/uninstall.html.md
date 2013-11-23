---
title: "Uninstall"
layout: "default"
category: "host"
menuOrder: 4
---

# Uninstall your Cozy
Warning: this will delete all your data.

## Easy way 

The easiest way to uninstall Cozy is to use the Fabric installation file used
for the installation. But this time, you should not call the installation task but
the uninstall task:

```bash
fab -H user@ip uninstall_all
```
   
## Manual uninstall

If you want to uninstall your Cozy without Fabric you can run the following
commands directly on your server.

```bash
supervisorctl stop cozy-controller
supervisorctl stop cozy-indexer
supervisorctl stop couchdb
rm -rf /usr/local/var/cozy-indexer
rm -f /etc/supervisor/conf.d/cozy-controller.conf
rm -f /etc/supervisor/conf.d/cozy-indexer.conf
rm -rf /usr/local/cozy
rm -rf /etc/cozy
apt-get purge supervisord
```
