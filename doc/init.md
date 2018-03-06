Sample init scripts and service configuration for testcurrencyd
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/testcurrencyd.service:    systemd service unit configuration
    contrib/init/testcurrencyd.openrc:     OpenRC compatible SysV style init script
    contrib/init/testcurrencyd.openrcconf: OpenRC conf.d file
    contrib/init/testcurrencyd.conf:       Upstart service configuration file
    contrib/init/testcurrencyd.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "testcurrency" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, testcurrencyd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, testcurrencyd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that testcurrencyd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If testcurrencyd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/testcurrency/testcurrency.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/testcurrency.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/testcurrencyd
Configuration file:  /etc/testcurrency/testcurrency.conf
Data directory:      /var/lib/testcurrencyd
PID file:            /var/run/testcurrencyd/testcurrencyd.pid (OpenRC and Upstart)
                     /var/lib/testcurrencyd/testcurrencyd.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by the testcurrency user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
testcurrency user and group.  Access to testcurrency-cli and other testcurrencyd rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start testcurrencyd" and to enable for system startup run
"systemctl enable testcurrencyd"

4b) OpenRC

Rename testcurrencyd.openrc to testcurrencyd and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/testcurrencyd start" and configure it to run on startup with
"rc-update add testcurrencyd"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop testcurrencyd.conf in /etc/init.  Test by running "service testcurrencyd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy testcurrencyd.init to /etc/init.d/testcurrencyd. Test by running "service testcurrencyd start".

Using this script, you can adjust the path and flags to the testcurrencyd program by
setting the TestcurrencyD and FLAGS environment variables in the file
/etc/sysconfig/testcurrencyd. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
