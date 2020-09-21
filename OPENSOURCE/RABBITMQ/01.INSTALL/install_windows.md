# Installing on Windows

## Overview

This guide covers RabbitMQ installation on Windows. It focuses on the two recommended installation options:

- Using Chocolatey
- Using the official installer

The guide also covers some Windows-specific aspects of managing the service.

## Using chocolatey

RabbitMQ packagese are distributed via Chocolatey. New releases can take a while (sometimes weeks) to get through approvals, so this option is not guaranteed to provide the latest release. It does, however, manage the required dependencies.

To install RabbitMQ, run the following command from the command line or from PowerShell:

```bash
choco install rabbitmq
```

For most users and use cases, Chocolatey is the optimal installation method.

## Using the Installer

The official RabbitMQ installer is produced for every RabbitMQ releases. It requires a dependency

### Dependencies

RabbitMQ requires a 64-bit supported version of Erlang for Windows to be installed. Erlang releases include a Windows installer. Erlang Solutions provide binary 64-bit builds of Erlang as well.

Important: the Erlang installer must be run using an administrative account otherwise a registry key expected by the RabbitMQ installer will not be present.

Important: your system should only have one version of Erlang installed. Please consult the Windows-specific Issues page.

Once a supported version of Erlang is installed, download the RabbitMQ installer (rabbitmq-server-3.8.3.exe) and run it. It installs RabbitMQ as a Windows service and starts it using the default configuration.

## Direct Downloads

Description	Download	Signature
Installer for Windows systems (from GitHub, recommended)	rabbitmq-server-3.8.3.exe	Signature
Alternative download location (from Bintray)	rabbitmq-server-3.8.3.exe	

## Run RabbitMQ Service

### Customise RabbitMQ Environment Variables

The service will run fine using its default settings. It is possible to customise the RabbitMQ environment or edit configuration.

Important: after setting environment variables, it is necessary to reinstall the service.

### Run RabbitMQ

The RabbitMQ service starts automatically. You can stop/reinstall/start the RabbitMQ service from the Start Menu.

## Upgrading Erlang VM

If you have an existing installation and are planning to upgrade the Erlang VM from a 32bit to a 64bit version then you must uninstall the broker before upgrading the VM. The installer will not be able to stop or remove a service that was installed with an Erlang VM of a different architecture.

## Managing a RabbitMQ Node

## Managing the Service

Links to RabbitMQ directories can be found in the Start Menu.

There is also a link to a command prompt window that will start in the sbin dir, in the Start Menu. This is the most convenient way to run the command line tools. Note that CLI tools will have to authenticate to the RabbitMQ node running locally. That involves a shared secret file which has to be placed into the correct location for the user.

### Stopping a Node

To stop the broker or check its status, use rabbitmqctl.bat in sbin (as an administrator).

```bash
rabbitmqctl.bat stop
```

### Checking Node Status

The following command performs the most basic node health check and displays some information about the node if it is running:

```bash
rabbitmqctl.bat status
```

See RabbitMQ CLI tools guide and the Monitoring and Health Checks guide for details.

## Log Files and Management

Server logs are critically important in troubleshooting and root cause analysis. See Logging and File and Directory Location guides to learn about log file location, log rotation and more.

### Troubleshooting When Running as a Service

In the event that the Erlang VM crashes whilst RabbitMQ is running as a service, rather than writing the crash dump to the current directory (which doesn't make sense for a service) it is written to an erl_crash.dump file in the base directory of the RabbitMQ server (set by the RABBITMQ_BASE environment variable, defaulting to %APPDATA%\%RABBITMQ_SERVICENAME% - typically %APPDATA%\RabbitMQ otherwise).

## Default User Access

The broker creates a user guest with password guest. Unconfigured clients will in general use these credentials. By default, these credentials can only be used when connecting to the broker as localhost so you will need to take action before connecting from any other machine.

See the documentation on access control for information on how to create more users and delete the guest user.

## Port Access

RabbitMQ nodes bind to ports (open server TCP sockets) in order to accept client and CLI tool connections. Other processes and tools such as anti-virus software may prevent RabbitMQ from binding to a port. When that happens, the node will fail to start.

CLI tools, client libraries and RabbitMQ nodes also open connections (client TCP sockets). Firewalls can prevent nodes and CLI tools from communicating with each other. Make sure the following ports are accessible:

- 4369: epmd, a peer discovery service used by RabbitMQ nodes and CLI tools
- 5672, 5671: used by AMQP 0-9-1 and 1.0 clients without and with TLS
- 25672: used for inter-node and CLI tools communication (Erlang distribution server port) and is allocated from a dynamic range (limited to a single port by default, computed as AMQP port + 20000). Unless external connections on these ports are really necessary (e.g. the cluster uses federation or CLI tools are used on machines outside the subnet), these ports should not be publicly exposed. See networking guide for details.
- 35672-35682: used by CLI tools (Erlang distribution client ports) for communication with nodes and is allocated from a dynamic range (computed as server distribution port + 10000 through server distribution port + 10010). See networking guide for details.
- 15672: HTTP API clients, management UI and rabbitmqadmin (only if the management plugin is enabled)
- 61613, 61614: STOMP clients without and with TLS (only if the STOMP plugin is enabled)
- 1883, 8883: (MQTT clients without and with TLS, if the MQTT plugin is enabled
- 15674: STOMP-over-WebSockets clients (only if the Web STOMP plugin is enabled)
- 15675: MQTT-over-WebSockets clients (only if the Web MQTT plugin is enabled)

It is possible to configure RabbitMQ to use different ports and specific network interfaces.

## Windows-specific Issues

We aim to make RabbitMQ a first-class citizen on Windows. However, sometimes there are circumstances beyond our control. Please consult the Windows-specific Issues page.

### Getting Help and Providing Feedback

If you have questions about the contents of this guide or any other topic related to RabbitMQ, don't hesitate to ask them on the RabbitMQ mailing list.

### Help Us Improve the Docs <3


If you'd like to contribute an improvement to the site, its source is available on GitHub. Simply fork the repository and submit a pull request. Thank you!