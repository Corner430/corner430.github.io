---
title: Delete the host key record for the specified IP address
date: 2023-04-03 19:11:02
tags:
---
- Deletes the host key record for the specified IP address from the SSH client's "known_hosts" file.
`ssh-keygen -f "~/.ssh/known_hosts" -R "172.17.0.158"`

If you need to specify the IP address of the SSH connection as port 8002 of 172.17.0.158, you can use the following command:
`ssh-keygen -f "~/.ssh/known_hosts" -R "[172.17.0.158]:8002"`