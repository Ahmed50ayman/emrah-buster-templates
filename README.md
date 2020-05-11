Table of contents
=================

- [About](#about)
- [Usage](#usage)
- [Example](#example)
- [Available templates](#available-templates)
    - [eb-base](#eb-base)
        - [To install eb-base](#to-install-eb-base)
    - [eb-livestream](#eb-livestream)
        - [Main components of eb-livestream](#main-components-of-eb-livestream)
        - [To install eb-livestream](#to-install-eb-livestream)
        - [After install eb-livestream](#after-install-eb-livestream)
        - [Related links to eb-livestream](#related-links-to-eb-livestream)
    - [eb-gitea](#eb-gitea)
        - [Main components of eb-gitea](#main-components-of-eb-gitea)
        - [To install eb-gitea](#to-install-eb-gitea)
        - [After install eb-gitea](#after-install-eb-gitea)
        - [Customizing eb-gitea](#customizing-eb-gitea)
    - [eb-jitsi](#eb-jitsi)
        - [Main components of eb-jitsi](#main-components-of-eb-jitsi)
        - [To install eb-jitsi](#to-install-eb-jitsi)
        - [Customizing eb-jitsi](#customizing-eb-jitsi)
        - [VideoBridge NAT config](#videobridge-nat-config)
- [Let's Encrypt support](#lets-encrypt-support)
- [Requirements](#requirements)

---

About
=====

`emrah-buster` is an installer to create the containerized systems on Debian
Buster host. It built on top of LXC (Linux containers). This repository
contains the `emrah-buster` templates.

Usage
=====

Download the installer, run it with a template name as an argument and drink a
coffee. That's it.

```bash
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-base/master/installer/eb
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-templates/master/installer/<TEMPLATE_NAME>.conf
bash eb <TEMPLATE_NAME>
```

Example
=======

To install a streaming media system, login a Debian Buster host as `root` and

```bash
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-base/master/installer/eb
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-templates/master/installer/eb-livestream.conf
bash eb eb-livestream
```

Available templates
===================

eb-base
-------

Install only a containerized Debian Buster.

### To install eb-base

```bash
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-base/master/installer/eb
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-templates/master/installer/eb-base.conf
bash eb eb-base
```

---

eb-livestream
-------------

Install a ready-to-use live streaming media system.

### Main components of eb-livestream

- Nginx server with nginx-rtmp-module as a stream origin.
  It gets the RTMP stream and convert it to HLS and DASH.

- Nginx server with standart modules as a stream edge.
  It publish the HLS and DASH stream.

- Web based HLS video player.

- Web based DASH video player.

### To install eb-livestream

```bash
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-base/master/installer/eb
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-templates/master/installer/eb-livestream.conf
bash eb eb-livestream
```

### After install eb-livestream

- `rtmp://<IP_ADDRESS>/livestream/<CHANNEL_NAME>` to push
   an RTMP stream.

- `http://<IP_ADDRESS>/livestream/hls/<CHANNEL_NAME>/index.m3u8` to pull
  the HLS stream.

- `http://<IP_ADDRESS>/livestream/dash/<CHANNEL_NAME>/index.mpd` to pull
  the DASH stream.

- `http://<IP_ADDRESS>/livestream/hlsplayer/<CHANNEL_NAME>` for
  the HLS video player page.

- `http://<IP_ADDRESS>/livestream/dashplayer/<CHANNEL_NAME>` for
  the DASH video player page.

- `http://<IP_ADDRESS>:8000/livestream/status` for the RTMP status page.

- `http://<IP_ADDRESS>:8000/livestream/cloner` for the stream cloner page.
  Thanks to [nejdetckenobi](https://github.com/nejdetckenobi)

### Related links to eb-livestream

- [nginx-rtmp-module](https://github.com/arut/nginx-rtmp-module)

- [video.js](https://github.com/videojs/video.js)

- [dash.js](https://github.com/Dash-Industry-Forum/dash.js/)

---

eb-gitea
-------------

Install a ready-to-use self-hosted Git service. Only AMD64 architecture is
supported for this template. Please contact me if you need eb-gitea for an
other architecture.

### Main components of eb-gitea

- [Gitea](https://gitea.io/)
- [Git](https://git-scm.com/)
- [MariaDB](https://mariadb.org/)
- [Nginx](http://nginx.org/)

### To install eb-gitea

```bash
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-base/master/installer/eb
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-templates/master/installer/eb-gitea.conf
bash eb eb-gitea
```

### After install eb-gitea
There is one more step to finish the installation. It's needed to fill
the initial configuration form of Gitea. Only two fields will be changed:
**SSH Server Domain** and **Gitea Base URL**.

Very easy!

- Access `https://<IP_ADDRESS>/install` to fill the Gitea config form.

- **SSH Server Domain**: Write your host FQDN or IP address. Examples:   
  *git.mydomain.com*   
  *123.2.3.4*

- **Gitea Base URL**: Write your URL. HTTP and HTTPS are OK. Examples:   
  *https://git.mydomain.com/*    
  *https://123.2.3.4/*

- Don't change the other fields. They are all OK.

- The first registered user will be the administrator.

### Customizing eb-gitea
Edit `/home/gitea/custom/conf/app.ini` file and restart the service to
customize Gitea. These are the customized settings of my private server.
See [cheat-sheet](https://docs.gitea.io/en-us/config-cheat-sheet/) for more
details.

- PASSWORD_COMPLEXITY = lower,upper,digit
- FORCE_PRIVATE = true
- DEFAULT_PRIVATE = true
- DISABLE_REGISTRATION = true
- REQUIRE_SIGNIN_VIEW = true
- DEFAULT_KEEP_EMAIL_PRIVATE = true
- [mailer] ENABLED = true
- [mailer] HOST = my-smtp-server.domain.com:587
- [mailer] FROM = emrah@domain.com
- [mailer] USER = emrah@domain.com
- [mailer] PASSWD = my-secret

---

eb-jitsi
--------

Install a ready-to-use self-hosted Jitsi/Jibri service. Jibri needs `snd_aloop`
kernel module, therefore it's not OK with the cloud kernel. Install the
standard Linux kernel if this is the case.

Thanks to [Ave](https://ave.zone/) and [Fatih](https://www.fatihmalakci.com/)
for their support.

### Main components of eb-jitsi

- [Jitsi](https://jitsi.org/)
- [Jibri](https://github.com/jitsi/jibri)
- [Nginx](http://nginx.org/)

### To install eb-jitsi

Download the installer

```bash
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-base/master/installer/eb
wget https://raw.githubusercontent.com/emrahcom/emrah-buster-templates/master/installer/eb-jitsi.conf
```

Open `eb-jitsi.conf` file with an editor and change the `JITSI_HOST` value.

```bash
vim eb-jitsi.conf
```

Use a resolvable host address.

```
export JITSI_HOST=jitsi.mydomain.com
```

And run the installer

```bash
bash eb eb-jitsi
```

### Customizing eb-jitsi
You may want to change the following values in
`/etc/jitsi/meet/YOUR_HOST_ADDRESS-config.js`. This file is on the
`eb-jitsi` container, not on the host.

- startAudioMuted
- resolution
- startVideoMuted
- requireDisplayName
- defaultLanguage

### VideoBridge NAT config
You need to update the videobridge NAT config if your host IP changed or the
recording property don't run properly.

`/etc/jitsi/videobridge/sip-communicator.properties` on `eb-jitsi` container

```
org.ice4j.ice.harvest.NAT_HARVESTER_PUBLIC_ADDRESS=<PUBLIC_IP_ADDRESS>
```

---

Let's Encrypt support
=====================

To use Let's Encrypt certificate, connect to the related container as root and

```bash
FQDN="your.host.fqdn"

certbot certonly --webroot -w /var/www/html -d $FQDN

chmod 750 /etc/letsencrypt/{archive,live}
chown root:ssl-cert /etc/letsencrypt/{archive,live}
rm -f /etc/ssl/certs/ssl-eb.pem
rm -f /etc/ssl/private/ssl-eb.key
ln -s /etc/letsencrypt/live/$FQDN/fullchain.pem \
    /etc/ssl/certs/ssl-eb.pem
ln -s /etc/letsencrypt/live/$FQDN/privkey.pem \
    /etc/ssl/private/ssl-eb.key

systemctl restart nginx.service
```

---

Requirements
============

`emrah-buster` requires a Debian Buster host with a minimal install and the
Internet access during the installation. It's not a good idea to use your
desktop machine or an already in-use production server as a host machine.
Please, use one of the followings as a host:

- a cloud host from a hosting/cloud service
  ([Digital Ocean](https://www.digitalocean.com/?refcode=92b0165840d8)'s
  droplet, [Amazon](https://console.aws.amazon.com) EC2 instance etc)

- a virtual machine (VMware, VirtualBox etc)

- a Debian Buster container

- a physical machine with a fresh installed [Debian Buster](https://www.debian.org/distrib/netinst)
