# Installing on Debian and Ubuntu

## Overview

This guide covers RabbitMQ installation on Debian, Ubuntu and distributions based on one of them.

RabbitMQ is included in standard Debian and Ubuntu repositories. However, the versions included are usually months or even years behind latest RabbitMQ releases, and thus are out of support.

RabbitMQ release artifacts include a Debian package. Team RabbitMQ also maintains our own apt repositories.

Main topics covered in this guide are

- Ways of installing the latest RabbitMQ version on Debian and Ubuntu
- Supported Ubuntu and Debian distributions
- How to install RabbitMQ using apt from Bintray or Package Cloud, including a quick start snippet
- How to install a recent supported Erlang version using apt
- Version Pinning of apt packages
- Privilege requirements
- Direct download from GitHub
- How to manage the service
- How to inspect node and service logs

and more.

## How to Install Latest RabbitMQ on Debian and Ubuntu

There are two ways to install the most recent version of RabbitMQ on Debian and Ubuntu:

- Using an apt repository on Package Cloud or Bintray (this option is highly recommended)
- Downloading the package and installing it manually with dpkg -i. This option will require manual installation of all package dependencies.

This guide covers both options. In both cases, a supported version of Erlang has to be installed. On Debian and Ubuntu, the easiest way to do that is via apt.

## Supported Distributions

Below is a list Debian-based distributions supported by recent RabbitMQ releases:

- Ubuntu 14.04 through 19.04
- Debian Stretch (9), Buster (10) and Sid ("unstable")

The package may work on other Debian-based distributions if dependencies are satisfied (e.g. using the Wheezy backports repository) but their testing and support is done on a best effort basis.

## Where to Get Recent Erlang Version on Debian and Ubuntu

RabbitMQ needs Erlang/OTP to run. Erlang/OTP packages in standard Debian and Ubuntu repositories can be significantly out of date and not supported by modern RabbitMQ versions.

Most recent Erlang/OTP release series are available from a number of alternative apt repositories:

Erlang Release Series	Repositories that provide it	Notes
22.x	
Debian packages of Erlang by Team RabbitMQ
Erlang Solutions
Supported starting with 3.7.15. See Erlang compatibility guide.
21.3.x	
Debian packages of Erlang by Team RabbitMQ
Erlang Solutions
Supported starting with 3.7.7. See Erlang compatibility guide.
20.3.x	
Debian packages of Erlang by Team RabbitMQ
Erlang Solutions
Supported starting with 3.6.11. See Erlang compatibility guide.
This guide will focus on the first option.

## Install Erlang from an Apt Repostory on Bintray

Standard Debian and Ubuntu repositories tend to provide outdated versions of Erlang/OTP. Team RabbitMQ maintains an apt repository that includes packages of modern Erlang/OTP releases for a number of commonly used Debian and Ubuntu distributions:

- Ubuntu 18.04 (Bionic)
- Ubuntu 16.04 (Xenial)
- Debian Stretch and Buster

The repo provides most recent patch releases in the following Erlang series:

- 22.x
- 21.x
- 20.3.x
- 19.3.x
- master (23.x)
- R16B03 (16.x)

In order to use the repository, it is necessary to

- Add (import) repository signing key. apt will verify package signatures during installation.
- Add a source list file for the repository
- Update package metadata
- Install Erlang packages required by RabbitMQ

## Add Repository Signing Key

In order to use the repository, add RabbitMQ signing key to apt-key. This will instruct apt to trust packages signed by that key. This can be done using a key server or via direct key download. Direct download is recommended as SKS key server are prone to overload:

```bash
curl -fsSL https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc | sudo apt-key add -
```

Using a key server:

```bash
sudo apt-key adv --keyserver "hkps://keys.openpgp.org" --recv-keys "0x0A9AF2115F4687BD29803A206B73A36E6026DFCA"
```

See the guide on signatures to learn more.

## Enable apt HTTPS Transport

In order for apt to be able to download RabbitMQ and Erlang packages from Bintray, the apt-transport-https package must be installed:

```bash
sudo apt-get install apt-transport-https
```

## Add a Source List File

As with all 3rd party Apt (Debian) repositories, a file describing the repository must be placed under the /etc/apt/sources.list.d/ directory. /etc/apt/sources.list.d/bintray.erlang.list is the recommended location.

The file should have a source (repository) definition line that uses the following pattern:

```bash
# This repository provides Erlang packages produced by the RabbitMQ team
# See below for supported distribution and component values
deb http://dl.bintray.com/rabbitmq-erlang/debian $distribution $component
```

The next couple of sections discuss what distribution and component values are supported.

### Distribution

In order to set up an apt repository that provides the correct package, a few decisions have to be made. One is determining the distribution name. It typically matches the Debian or Ubuntu release used but only a handful of distributions are supported (indexed) by the Erlang Debian packages maintained by Team RabbitMQ:

bionic for Ubuntu 18.04
xenial for Ubuntu 16.04
buster for Debian Buster
stretch for Debian Stretch
However, not all distributions are covered (indexed) on Bintray. But there are good news: since the package indexed for these distributions is identical, any reasonably recent distribution name would suffice in practice. For example, users of Debian Buster, Debian Sid, Ubuntu Disco and Ubuntu Eoan can use both stretch and bionic for distribution name.

Below is a table of OS release and distribution names that should be used on Bintray.

Release	Distribution Name to Use on Bintray
Ubuntu 18.04	bionic
Ubuntu 19.04	bionic
Ubuntu 19.10	bionic
Ubuntu 16.04	xenial
Debian Buster	buster
Debian Stretch	stretch
Debian Sid	buster
Erlang/OTP Version
Another is what Erlang/OTP release version should be provisioned. It is possible to track a specific series (e.g. 21.x) or install the most recent version available. The choice determines what Debian repository component will be configured.

Consider the following repository file at /etc/apt/sources.list.d/bintray.erlang.list:

## Installs the latest 21.x version available in the repository.
## Please see the distribution name table above.
deb http://dl.bintray.com/rabbitmq-erlang/debian bionic erlang-21.x
It configures apt to install the most recent Erlang 21.x version available in the repository and use packages for Ubuntu 18.04 (Bionic). More recent Ubuntu releases should also use this distribution name.

For Debian Buster the file would look like this:

## Installs the latest 21.x version available in the repository.
## Please see the distribution name table above.
deb http://dl.bintray.com/rabbitmq-erlang/debian buster erlang-21.x
For Debian Stretch:

## Installs the latest 21.x version available in the repository.
## Please see the distribution name table above.
deb http://dl.bintray.com/rabbitmq-erlang/debian stretch erlang-21.x
To use the most recent 20.x patch release available, switch the component to erlang-20.x:

## Installs the latest 20.x version available in the repository.
## Please see the distribution name table above.
deb http://dl.bintray.com/rabbitmq-erlang/debian bionic erlang-20.x
erlang-21.x, erlang-19.x, and erlang-16.x are the components for Erlang 21.x, 19.x and R16B03, respectively.

The erlang component installs the most recent version available:

## Installs the latest version available in the repository.
## Consider using version pinning.
## Please see the distribution name table above.
deb http://dl.bintray.com/rabbitmq-erlang/debian bionic erlang
That version may or may not be supported by RabbitMQ, so package version pinning is highly recommended.

Install Erlang Packages
After updating the list of apt sources it is necessary to run apt-get update:

sudo apt-get update -y
Then packages can be installed just like with the standard Debian repositories:

# This is recommended. Metapackages such as erlang and erlang-nox must only be used
# with apt version pinning. They do not pin their dependency versions.
sudo apt-get install -y erlang-base \
                        erlang-asn1 erlang-crypto erlang-eldap erlang-ftp erlang-inets \
                        erlang-mnesia erlang-os-mon erlang-parsetools erlang-public-key \
                        erlang-runtime-tools erlang-snmp erlang-ssl \
                        erlang-syntax-tools erlang-tftp erlang-tools erlang-xmerl
Package Version and Repository Pinning
When the same package (e.g. erlang-base) is available from multiple apt repositories operators need to have a way to indicate what repository should be preferred. It may also be desired to restrict Erlang version to avoid undesired upgrades. apt package pinning feature can be used to address both problems.

Package pinning is configured with a file placed under the /etc/apt/preferences.d/ directory, e.g. /etc/apt/preferences.d/erlang. After updating apt preferences it is necessary to run apt-get update:

sudo apt-get update -y
The following preference file example will configure apt to install erlang-* packages from Bintray and not standard Debian or Ubuntu repository:

# /etc/apt/preferences.d/erlang
Package: erlang*
Pin: release o=Bintray
Pin-Priority: 1000
This apt preference configuration is recommended when the erlang repository component is used.

Effective package pinning policy can be verified with

sudo apt-cache policy
The following preference file example will pin all erlang-* packages to 22.3 (assuming package epoch for the package is 1):

# /etc/apt/preferences.d/erlang
Package: erlang*
Pin: version 1:22.3-1
Pin-Priority: 1000
The following preference file example will pin rabbitmq-server package to to 3.8.3 (assuming package epoch for the package is 1):

# /etc/apt/preferences.d/rabbitmq
Package: rabbitmq-server
Pin: version 1:3.8.3
Pin-Priority: 1000
In the example below, the esl-erlang package is pinned to to to 22.3 (assuming package epoch for the package is 1):

# /etc/apt/preferences.d/erlang
Package: esl-erlang
Pin: version 1:22.3
Pin-Priority: 1000
Using RabbitMQ Apt Repositories
RabbitMQ packages can be installed from apt (Debian) repositories on Package Cloud or Bintray. Both repositories provide packages for most recent RabbitMQ releases.

PackageCloud provides a more opinionated and automated way of repository setup. With Bintray the experience is closer to setting up any other 3rd party apt repository.

Using RabbitMQ Apt Repository on PackageCloud
PackageCloud is a package hosting service. Team RabbitMQ maintains an apt repository on PackageCloud.

A quick way to install uses a Package Cloud-provided script.

There are more installation options available:

Using PackageCloud Chef cookbook
Using PackageCloud Puppet module
Manually
PackageCloud RabbitMQ repository instructions lists all available options.

Package Cloud signs distributed packages using their own GPG keys.

# import PackageCloud signing key
wget -O - "https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey" | sudo apt-key add -
After importing both keys please follow the Package Cloud repository setup instructions.

Using RabbitMQ Apt Repository on Bintray
Bintray is a package distribution service. Team RabbitMQ maintains an apt repository on Bintray. When using the repository on Bintray it is recommended that Erlang/OTP is also installed from Bintray.

In order to use a 3rd apt repository, it is necessary to

Add (import) repository signing key. apt will verify package signatures during installation.
Add a repository file
Update package metadata
Install the Erlang package
Quick Start Example
Below is shell snippet that performs those steps. They are documented in more detail below.

#!/bin/sh

## If sudo is not available on the system,
## uncomment the line below to install it
# apt-get install -y sudo

sudo apt-get update -y

## Install prerequisites
sudo apt-get install curl gnupg -y

## Install RabbitMQ signing key
curl -fsSL https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc | sudo apt-key add -

## Install apt HTTPS transport
sudo apt-get install apt-transport-https

## Add Bintray repositories that provision latest RabbitMQ and Erlang 21.x releases
sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list <<EOF
## Installs the latest Erlang 22.x release.
## Change component to "erlang-21.x" to install the latest 21.x version.
## "bionic" as distribution name should work for any later Ubuntu or Debian release.
## See the release to distribution mapping table in RabbitMQ doc guides to learn more.
deb https://dl.bintray.com/rabbitmq-erlang/debian bionic erlang
deb https://dl.bintray.com/rabbitmq/debian bionic main
EOF

## Update package indices
sudo apt-get update -y

## Install rabbitmq-server and its dependencies
sudo apt-get install rabbitmq-server -y --fix-missing
Add Signing Key
See Add Repository Signing Key above.

Enable apt HTTPS Transport
See Install apt HTTPS transport above.

Add a Source List File
As with all 3rd party apt repositories, a file describing the RabbitMQ and Erlang package repositories must be placed under the /etc/apt/sources.list.d/ directory. /etc/apt/sources.list.d/bintray.rabbitmq.list is the recommended location.

The file should have a source (repository) definition line that uses the following pattern:

# Source repository definition example.
# See below for supported distribution and component values

# Use this line to install the latest Erlang 21.3.x package available
deb https://dl.bintray.com/rabbitmq-erlang/debian $distribution erlang-21.x

# Or use this line to install the latest Erlang 22.x package available
# deb https://dl.bintray.com/rabbitmq-erlang/debian $distribution erlang

# This repository provides RabbitMQ packages
deb https://dl.bintray.com/rabbitmq/debian $distribution main
The next couple of sections discusses what distribution and component values are supported.

Distribution
In order to set up an apt repository that provides the correct package, a few decisions have to be made. One is determining the distribution name. It typically matches the Debian or Ubuntu release used:

bionic for Ubuntu 18.04
xenial for Ubuntu 16.04
buster for Debian Buster
stretch for Debian Stretch
However, not all distributions are covered (indexed) on Bintray. But there are good news: since the package indexed for these distributions is identical, any reasonably recent distribution name would suffice in practice. For example, users of Debian Buster, Debian Sid, Ubuntu Disco and Ubuntu Eoan can use both stretch and bionic for distribution name.

Below is a table of OS release and distribution names that should be used on Bintray.

Release	Distribution Name to Use on Bintray
Ubuntu 18.04	bionic
Ubuntu 19.04	bionic
Ubuntu 19.10	bionic
Ubuntu 16.04	xenial
Debian Buster	stretch
Debian Stretch	stretch
Debian Sid	stretch
To add the apt repository to the source list directory (/etc/apt/sources.list.d), use:

echo "deb https://dl.bintray.com/rabbitmq/debian {distribution} main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
where {distribution} is the name of the Debian or Ubuntu distribution used (see the table above).

So, on Ubuntu 18.04 and later releases the above command becomes

echo "deb https://dl.bintray.com/rabbitmq/debian bionic main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
bionic for distribution name would also work on Debian Buster and Debian Sid.

On Debian Stretch it would be

echo "deb https://dl.bintray.com/rabbitmq/debian stretch main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
stretch for distribution name would also work on Debian Buster.

On Ubuntu 16.04 it would be

echo "deb https://dl.bintray.com/rabbitmq/debian xenial main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
It is possible to list multiple repositories, for example, one that provides RabbitMQ and one that provides Erlang/OTP packages. On Ubuntu 18.04 that can be done by modifying the command in the above example like so:

sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list <<EOF
deb https://dl.bintray.com/rabbitmq-erlang/debian bionic erlang
deb https://dl.bintray.com/rabbitmq/debian bionic main
EOF
and on Ubuntu 16.04 it would be

sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list <<EOF
deb https://dl.bintray.com/rabbitmq-erlang/debian xenial erlang
deb https://dl.bintray.com/rabbitmq/debian xenial main
EOF
Install RabbitMQ Package
After updating the list of apt sources it is necessary to run apt-get update:

sudo apt-get update -y
Then install the package with

sudo apt-get install -y rabbitmq-server
Manual Installation with Dpkg
In some cases it may easier to download the package directly from GitHub and install it manually using sudo dpkg -i. Below is a download link.

Description	Download	Signature
.deb for Debian-based Linux (from GitHub)	rabbitmq-server_3.8.3-1_all.deb	Signature
When installing manually with dpkg, it is necessary to install package dependencies first. dpkg, unlike apt, does not resolve or manage dependencies.

Here's an example that does that, installs wget, downloads the RabbitMQ package and installs it:

# sync package metadata
sudo apt-get update
# install dependencies manually
sudo apt-get -y install socat logrotate init-system-helpers adduser

# download the package
sudo apt-get -y install wget
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.3/rabbitmq-server_3.8.3-1_all.deb

# install the package with dpkg
sudo dpkg -i rabbitmq-server_3.8.3-1_all.deb

rm rabbitmq-server_3.8.3-1_all.deb
Installation via apt repositories on Bintray and Package Cloud is recommended over downloading the package directly and installing via dpkg -i. When the RabbitMQ package is installed manually with dpkg -i the operator is responsible for making sure that all package dependencies are met.

User Privilege Requirements
RabbitMQ Debian package will require sudo privileges to install and manage. In environments where sudo isn't available, consider using the generic binary build instead.

Run RabbitMQ Server
Start the Server
The server is started as a daemon by default when the RabbitMQ server package is installed. It will run as a non-privileged user rabbitmq.

As an administrator, start and stop the server as usual for Debian-based systems: service rabbitmq-server start.

Configuring RabbitMQ
On most systems, a node should be able to start and run with all defaults. Please refer to the Configuration guide to learn more and Production Checklist for guidelines beyond development environments.

Note: the node is set up to run as system user rabbitmq. If location of the node database or the logs is changed, the files and directories must be owned by this user.

Port Access
RabbitMQ nodes bind to ports (open server TCP sockets) in order to accept client and CLI tool connections. Other processes and tools such as SELinux may prevent RabbitMQ from binding to a port. When that happens, the node will fail to start.

CLI tools, client libraries and RabbitMQ nodes also open connections (client TCP sockets). Firewalls can prevent nodes and CLI tools from communicating with each other. Make sure the following ports are accessible:

4369: epmd, a peer discovery service used by RabbitMQ nodes and CLI tools
5672, 5671: used by AMQP 0-9-1 and 1.0 clients without and with TLS
25672: used for inter-node and CLI tools communication (Erlang distribution server port) and is allocated from a dynamic range (limited to a single port by default, computed as AMQP port + 20000). Unless external connections on these ports are really necessary (e.g. the cluster uses federation or CLI tools are used on machines outside the subnet), these ports should not be publicly exposed. See networking guide for details.
35672-35682: used by CLI tools (Erlang distribution client ports) for communication with nodes and is allocated from a dynamic range (computed as server distribution port + 10000 through server distribution port + 10010). See networking guide for details.
15672: HTTP API clients, management UI and rabbitmqadmin (only if the management plugin is enabled)
61613, 61614: STOMP clients without and with TLS (only if the STOMP plugin is enabled)
1883, 8883: (MQTT clients without and with TLS, if the MQTT plugin is enabled
15674: STOMP-over-WebSockets clients (only if the Web STOMP plugin is enabled)
15675: MQTT-over-WebSockets clients (only if the Web MQTT plugin is enabled)
15692: Prometheus metrics (only if the Prometheus plugin is enabled)
It is possible to configure RabbitMQ to use different ports and specific network interfaces.

Default User Access
The broker creates a user guest with password guest. Unconfigured clients will in general use these credentials. By default, these credentials can only be used when connecting to the broker as localhost so you will need to take action before connecting from any other machine.

See the documentation on access control for information on how to create more users and delete the guest user.

Controlling System Limits on Linux
RabbitMQ installations running production workloads may need system limits and kernel parameters tuning in order to handle a decent number of concurrent connections and queues. The main setting that needs adjustment is the max number of open files, also known as ulimit -n. The default value on many operating systems is too low for a messaging broker (1024 on several Linux distributions). We recommend allowing for at least 65536 file descriptors for user rabbitmq in production environments. 4096 should be sufficient for many development workloads.

There are two limits in play: the maximum number of open files the OS kernel allows (fs.file-max) and the per-user limit (ulimit -n). The former must be higher than the latter.

With systemd (Recent Linux Distributions)
On distributions that use systemd, the OS limits are controlled via a configuration file at /etc/systemd/system/rabbitmq-server.service.d/limits.conf. For example, to set the max open file handle limit (nofile) to 64000:

[Service]
LimitNOFILE=64000
See systemd documentation to learn about the supported limits and other directives.

With Docker
To configure kernel limits for Docker contains, use the "default-ulimits" key in Docker daemon configuration file. The file has to be installed on Docker hosts at /etc/docker/daemon.json:

{
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 64000,
      "Soft": 64000
    }
  }
}
Without systemd (Older Linux Distributions)
The most straightforward way to adjust the per-user limit for RabbitMQ on distributions that do not use systemd is to edit the /etc/default/rabbitmq-server (provided by the RabbitMQ Debian package) or rabbitmq-env.conf to invoke ulimit before the service is started.

ulimit -S -n 4096
This soft limit cannot go higher than the hard limit (which defaults to 4096 in many distributions). The hard limit can be increased via /etc/security/limits.conf. This also requires enabling the pam_limits.so module and re-login or reboot. Note that limits cannot be changed for running OS processes.

For more information about controlling fs.file-max with sysctl, please refer to the excellent Riak guide on open file limit tuning.

Verifying the Limit
RabbitMQ management UI displays the number of file descriptors available for it to use on the Overview tab.

rabbitmqctl status
includes the same value.

The following command

cat /proc/$RABBITMQ_BEAM_PROCESS_PID/limits
can be used to display effective limits of a running process. $RABBITMQ_BEAM_PROCESS_PID is the OS PID of the Erlang VM running RabbitMQ, as returned by rabbitmqctl status.

Configuration Management Tools
Configuration management tools (e.g. Chef, Puppet, BOSH) provide assistance with system limit tuning. Our developer tools guide lists relevant modules and projects.

Managing the Service
To start and stop the server, use the service tool. The service name is rabbitmq-server:

# stop the local node
sudo service rabbitmq-server stop

# start it back
sudo service rabbitmq-server start
service rabbitmq-server status will report service status as observed by systemd (or similar service manager):

# check on service status as observed by service manager
sudo service rabbitmq-server status
It will produce output similar to this:

Redirecting to /bin/systemctl status rabbitmq-server.service
● rabbitmq-server.service - RabbitMQ broker
   Loaded: loaded (/usr/lib/systemd/system/rabbitmq-server.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/rabbitmq-server.service.d
           └─limits.conf
   Active: active (running) since Wed 2018-12-26 10:21:32 UTC; 25s ago
 Main PID: 957 (beam.smp)
   Status: "Initialized"
   CGroup: /system.slice/rabbitmq-server.service
           ├─ 957 /usr/lib/erlang/erts-10.2/bin/beam.smp -W w -A 64 -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -K true -- -root /usr/lib/erlang -progname erl -- -home /var/lib/rabbitmq -- ...
           ├─1411 /usr/lib/erlang/erts-10.2/bin/epmd -daemon
           ├─1605 erl_child_setup 400000
           ├─2860 inet_gethost 4
           └─2861 inet_gethost 4

Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##  ##
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##  ##      RabbitMQ 3.7.24. Copyright (c) 2007-2020 VMware, Inc. or its affiliates.
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##########  Licensed under the MPL.  See https://www.rabbitmq.com/
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ######  ##
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: ##########  Logs: /var/log/rabbitmq/rabbit@localhost.log
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: /var/log/rabbitmq/rabbit@localhost_upgrade.log
Dec 26 10:21:30 localhost.localdomain rabbitmq-server[957]: Starting broker...
Dec 26 10:21:32 localhost.localdomain rabbitmq-server[957]: systemd unit for activation check: "rabbitmq-server.service"
Dec 26 10:21:32 localhost.localdomain systemd[1]: Started RabbitMQ broker.
Dec 26 10:21:32 localhost.localdomain rabbitmq-server[957]: completed with 6 plugins.
rabbitmqctl, rabbitmq-diagnostics, and other CLI tools will be available in PATH and can be invoked by a sudo-enabled user:

# checks if the local node is running and CLI tools can successfully authenticate with it
sudo rabbitmq-diagnostics ping

# prints enabled components (applications), TCP listeners, memory usage breakdown, alarms
# and so on
sudo rabbitmq-diagnostics status

# prints effective node configuration
sudo rabbitmq-diagnostics environment

# performs a more extensive health check of the local node
sudo rabbitmq-diagnostics node_health_check
All rabbitmqctl commands will report the node absence if no broker is running.

See the CLI tools guide to learn more.

Log Files and Management
Server logs can be found under the configurable directory, which usually defaults to /var/log/rabbitmq when RabbitMQ is installed via a Linux package manager.

RABBITMQ_LOG_BASE can be used to override log directory location.

Assuming a systemd-based distribution, system service logs can be inspected using

journalctl --system
which requires superuser privileges. Its output can be filtered to narrow it down to RabbitMQ-specific entries:

sudo journalctl --system | grep rabbitmq
The output will look similar to this:

Dec 26 11:03:04 localhost rabbitmq-server[968]: ##  ##
Dec 26 11:03:04 localhost rabbitmq-server[968]: ##  ##      RabbitMQ 3.7.24. Copyright (c) 2007-2020 VMware, Inc. or its affiliates.
Dec 26 11:03:04 localhost rabbitmq-server[968]: ##########  Licensed under the MPL.  See https://www.rabbitmq.com/
Dec 26 11:03:04 localhost rabbitmq-server[968]: ######  ##
Dec 26 11:03:04 localhost rabbitmq-server[968]: ##########  Logs: /var/log/rabbitmq/rabbit@localhost.log
Dec 26 11:03:04 localhost rabbitmq-server[968]: /var/log/rabbitmq/rabbit@localhost_upgrade.log
Dec 26 11:03:04 localhost rabbitmq-server[968]: Starting broker...
Dec 26 11:03:05 localhost rabbitmq-server[968]: systemd unit for activation check: "rabbitmq-server.service"
Dec 26 11:03:06 localhost rabbitmq-server[968]: completed with 6 plugins.
Log Rotation
The broker always appends to the log files, so a complete log history is retained.

logrotate is the recommended way of log file rotation and compression. By default, the package will set up logrotate to run weekly on files located in default /var/log/rabbitmq directory. Rotation configuration can be found in /etc/logrotate.d/rabbitmq-server.

Getting Help and Providing Feedback
If you have questions about the contents of this guide or any other topic related to RabbitMQ, don't hesitate to ask them on the RabbitMQ mailing list.

Help Us Improve the Docs <3
If you'd like to contribute an improvement to the site, its source is available on GitHub. Simply fork the repository and submit a pull request. Thank you!