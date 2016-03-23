---
layout: post
title: SSH not support dsa and rsa keys
---

After upgrading my system and recovering my keys, I cannot ssh to any other servers with my private key. I check the keys over and over again, there's no reason the keys are wrong.

Google some articles, I find the problem. My `ssh-dsa` key type is `ssh-dss`, and it's disabled by default in openssh version 7.0 and above.

To solve this issue, just add new option in `ssh_config` 

`PubkeyAcceptedKeyTypes +ssh-dss`

This configuration makes my ssh accept DSA keys and I can ssh to my servers with my keys again.
