# System Scripts

The following are scripts that don't yet exist with various software packages.  I guess the developers are so brilliant, their brains are full.  So, here we are today.

##mongod.service##

This is a systemd unit to manage mongod via systemctl, instead of relying on it to forward to init.d which I've found doesn't always properly clean things up when you reboot a Fedora server, causing it to not load back up properly.  Stale lock files, etc.

Anyway, I prefer to stick to one system of managing my services, and I want it done consistently.

###Install###
    cp mongod.service /lib/systemd/system/

###Usage###
    systemctl enable mongod.service
    systemctl start mongod.service

##mongod@.service##

This is a systemd unit template to manage mongod via systemctl, allowing multiple instances of mongod to run on a single host.

###Install###
    cp mongod\@.service /lib/systemd/system/

###Usage###
    systemctl start mongod@<instance name>.service

###Gotchas###
On Fedora 16 through 20 (I'm assuming), enabling services using unit templates doesn't work.  They claim to have fixed it in Fedora 19, but they really haven't.

    ln -s /lib/systemd/system/mongod\@.service /etc/systemd/system/multi-user.target.wants/mongod\@<instance name>.service

Be sure to replace "<instance name>" with the name that maps to your mongodb configuration file for this instance, e.g. /etc/mongod-something.conf.
