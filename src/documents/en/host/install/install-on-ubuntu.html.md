---
title: "on Ubuntu"
layout: "default"
category: "host"
menuOrder: 0
toc: false
---


<div style="height: 0; overflow: shown; text-align: right">
<img src="/assets/images/ubuntu-logo.png">
</div>

# Install Cozy on Ubuntu

You need **Ubuntu 14.04 Trusty Tahr** in order to proceed to theses steps.

<br>

1. Make sure that `ca-certificates` and `apt-transport-https` are properly installed
    ```
    sudo apt-get install ca-certificates apt-transport-https
    ```
2. Import the repository public key
    ```
    wget -O - http://ubuntu.cozycloud.cc/cozy.gpg.key 2>/dev/null | sudo apt-key add -
    ```
3. Add the Cozy repository to your software sources
    ```
    sudo echo 'deb [arch=amd64] http://ubuntu.cozycloud.cc/debian trusty main' \
    > /etc/apt/sources.list.d/cozy.list
    ```
4. Install Cozy
    ```
    sudo apt-get update
    sudo apt-get install cozy
    ```
