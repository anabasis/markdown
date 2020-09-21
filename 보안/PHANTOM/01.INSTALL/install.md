## © 2019 Splunk Inc. All rights reserved.

- Installation overview
- Get Splunk Phantom
   - Phantom community account
   - AWS Marketplace
   - Where to get help
- System requirements
   - Supported operating systems
   - Supported browsers
   - Supported file systems and required directories
   - System requirements for evaluation use
   - System requirements for production use
   - Splunk Phantom required ports
      - Standalone Splunk Phantom instance
      - Splunk Phantom cluster node
      - Shared services for clustered environments
         - All Splunk Phantom nodes
         - Internode communications
         - File share
         - Embedded Splunk Enterprise
         - PostgreSQL
      - Mobile Device Registration
- Install Splunk Phantom using the Amazon Marketplace Image
   - Prerequisites
   - Installation
   - Log in to the Splunk Phantom web interface
- Install Splunk Phantom as a virtual appliance
   - Install Splunk Phantom with VMware vSphere ESXi or VMware vSphere
   - Install Splunk Phantom with VMware Fusion® or VMware Fusion Pro®
   - Install Splunk Phantom with VMware Workstation Pro®
   - Install Splunk Phantom with VMware Workstation Player®
   - Install Splunk Phantom with Oracle® VirtualBox
   - Complete the Splunk Phantom OVA install
      - Set operating system passwords
      - Assign an IP address to the virtual appliance
      - Log in to the Splunk Phantom web interface
            - Page 1 of
- Install Splunk Phantom using RPM
   - Prerequisites
      - Add the required additional YUM repositories
      - Update the operating system and dependencies
   - Install Splunk Phantom
   - Log in to the Splunk Phantom web interface
- Install Splunk Phantom on a system with limited internet access
   - Prerequisites
      - Update the operating system and dependencies
   - Install Splunk Phantom from the tar file
   - Log in to the Splunk Phantom web interface
- Install Splunk Phantom as an unprivileged user
   - Prerequisites
   - Prepare the system
      - Install the operating system dependencies
      - Create the user account that will run Splunk Phantom
   - Install Splunk Phantom from the tar file
   - Log in to the Splunk Phantom web interface
- Create a Splunk Phantom cluster
   - Create a Splunk Phantom Cluster from OVA installs
      - Build a cluster with a single Shared Services node
         - Checklist - Single Shared Services node
      - Build a cluster with external service services
         - Checklist - virtual machine images cluster with external services
   - Create a Splunk Phantom Cluster from rpm or tar file installs
         - Checklist - cluster with external services
   - Create a Splunk Phantom cluster using unprivileged installs
         - Checklist - unprivileged install cluster with external services
   - Run make_server_node.pyc
      - Additional configuration steps for unprivileged clusters
      - Create a Shared Services node
      - Create a specific function node
      - make_sever_node.pyc prompts and warnings
   - Run make_cluster_node.pyc
      - Collect the required information
      - Create a Splunk Phantom node
            - Page 2 of
         - Privileged installation
         - Unprivileged installation
   - Create a Splunk Phantom Cluster in Amazon Web Services
      - Prerequisites
         - Checklist - AMI-based cluster, using AWS services
      - Launch and prepare AMI-based instances of Splunk Phantom
      - Installation
         - Install support for Elastic File System (EFS)
         - Install SSH keys
         - Replace database preparation scripts
      - Create a load balancer with Elastic Load Balancer (ELB)
         - Create a Target Group to be used by your load balancer
         - Add the routing rules to your load balancer
      - Create the file stores with Elastic File System (EFS)
      - Create the external PostgreSQL database with the Relational Database System (RDS)
      - Create the pgbouncer user for the RDS
      - Add file shares to each Splunk Phantom instance
      - Convert an AMI-based Splunk Phantom instance into the Splunk Enterprise instance
      - Test each Splunk Phantom instance for readiness
      - Convert the first AMI-Based Splunk Phantom instance into a cluster node
         - Run make_cluster_node.pyc
      - Convert the remaining AMI-based Splunk Phantom instances into cluster nodes
         - Run make_cluster_node.pyc
      - Log in to the Splunk Phantom web interface
- Run Splunk Phantom using external services
   - Set up the external PostgreSQL server
   - Set up external file shares using GlusterFS
      - Prepare the GlusterFS server
      - Prepare TLS certificates
      - Configure the shared volumes
      - Configure Splunk Phantom cluster nodes to connect to the GlusterFS file shares
      - Sync Splunk Phantom cluster nodes to the shared volumes
   - Set up a load balancer with an HAProxy® server
   - Set up Splunk Enterprise
      - Install Splunk Enterprise and add-ons
      - Create required user accounts for Splunk Phantom
            - Page 3 of
      - Configure Splunk Phantom instances to use external Splunk Enterprise
      - Configure Splunk Phantom to use a distributed Splunk Enterprise deployment
- Upgrade Splunk Phantom
   - Prerequisites
      - Suggested preparations
   - Upgrade a standalone Splunk Phantom instance
   - Upgrade a Splunk Phantom cluster
      - Upgrade the operating system and installed packages
      - Install the Splunk Phantom repositories and signing keys
      - Upgrade individual cluster nodes
   - Upgrade a standalone, unprivileged Splunk Phantom instance
   - Upgrade an unprivileged Splunk Phantom cluster
      - Upgrade the operating system and installed packages
      - Copy the installation tar file to the cluster node
      - Upgrade individual cluster nodes
- Reference
   - Default credentials
   - Installation or configuration scripts
      - phantom_tar_install.sh options
      - Phantom_setup.sh options
      - make_server_node.pyc options
      - make_cluster_node.pyc options
   - Configuration files
      - HAProxy Configuration
         - Page 4 of


## Installation overview

**Applies to versions: 4.**

Installing Splunk Phantom is the first step to realizing gains from security orchestration and
automation.

There are several ways to install Splunk Phantom.
● Install Splunk Phantom with a machine image in the AWS Marketplace.
● Install Splunk Phantom as a virtual machine image.
● Install Splunk Phantom using an installation package.

Work with your Splunk Phantom Delivery Team representative to choose the right method for
your organization.

## Get Splunk Phantom

To get Splunk Phantom, you must do one of the following:
● Register a Phantom community account
● Buy an AWS Marketplace machine image

### Phantom community account

To get Splunk Phantom, you must have a Splunk Phantom Community account. Visit the ​Splunk
Phantom Community​ to register. Once your account has been approved, download virtual
machine images or other installation packages from the Product link.

If you don’t see the installation package you need, contact your sales or delivery team
representative.

### AWS Marketplace

Install Splunk Phantom for AWS from the AWS Marketplace in the security category.

```
Page 5 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Where to get help

If you need help installing Splunk Phantom several resources are available.
● Your Splunk Phantom Delivery representative
● The Splunk Phantom Community site at ​https://my.phantom.us/​ for documentation, the
support knowledge base, and video tutorials.
● The Splunk Phantom Community Slack instance at
https://phantom-community.slack.com​.
● Open a support case at ​https://support.splunk.com​ or by calling +1(855)SPLUNK-S or
+1(855)775-8657.
International Splunk Support numbers are located at
https://www.splunk.com/en_us/about-us/contact.html#tabs/customer-support​.

## System requirements

Splunk Phantom requires certain minimum system requirements. Your environment must meet
or exceed these requirements. This section details operating systems, web browsers, system
storage, Linux file systems, and other requirements for operating Splunk Phantom.

### Supported operating systems

Splunk Phantom supports these operating systems and versions:
● Red Hat Enterprise Linux 6.
● Red Hat Enterprise Linux 7.
● CentOS 6.
● CentOS 7.

### Supported browsers

Use the latest, fully patched version of your browser. Splunk Phantom requires a web browser
that supports HTML 5, SVG graphics, and TLS.

Splunk Phantom supports these web browsers:
● Google Chrome
● Mozilla Firefox
● Microsoft Internet Explorer 11
● Microsoft Edge
● Safari

```
Page 6 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Supported file systems and required directories

Splunk Phantom supports any file system where the user account running the application can
be given write permissions.

In a clustered environment, Splunk Phantom implements GlusterFS for its file shares. If your
organization requires a different file system for your Splunk Phantom cluster, make sure that the
user account running Splunk Phantom has write permissions to the required directories.

Required directories for a standard installation:
● /opt/phantom/apps
● /opt/phantom/local_data/app_states
● /opt/phantom/scm
● /opt/phantom/vault
● /opt/phantom/tmp/shared/

Required directories for an installation as an unprivileged user:
● <phantom_install_dir>/apps
● <phantom_install_dir>/local_data/app_states
● <phantom_install_dir>/scm
● <phantom_install_dir>/vault
● <phantom_install_dir>/tmp/shared

### System requirements for evaluation use

Your evaluation system must meet or exceed the listed requirements:
Hypervisor
for virtual
machine
images.

```
VMware Fusion
VMware Workstation
VMware Player
Oracle​ ​VirtualBox
Processor 1 CPU with a minimum of 4 cores.
Memory Minimum 8GB RAM, recommended 16GB
Storage Minimum 500GB of disk space.
Disk space requirements vary based on the volume of data consumed and
the size of your evaluation environment.
Network 1 network interface
```
```
Page 7 of 84
© 2019 Splunk Inc. All rights reserved.
```

### System requirements for production use

Systems for production must meet or exceed the listed requirements:
Hypervisor
for virtual
machine
images.

```
VMware vSphere ESX/ESXi 5 or later
```
```
Processor 1 server-class CPU, 4 to 8 cores
Memory Minimum of 16GB RAM, 32GB recommended
Storage Splunk Phantom needs storage for multiple volumes:
● PostgreSQL database: 500GB
● Embedded Splunk Enterprise: 500GB
● File share volumes: 500GB
```
```
Storage requirements will vary based on the volume of data consumed and
the size of your enterprise.
Network A one-gigabit network interface
```
**Caution:** ​ You might need a larger volume of storage. Using the Splunk Phantom Files feature to
store virtual machine snapshots or other large-format data consumes significant storage.

### Splunk Phantom required ports

These tables list the ports which must be open to traffic in order to use Splunk Phantom. Use
these tables to design your firewall rules for your installation.

**Note:** ​ Some Splunk Phantom apps might require additional ports. Consult the individual app
descriptions for additional information.

#### Standalone Splunk Phantom instance

On a single instance of Splunk Phantom, where all services are contained on the same host,
open these ports.
**Port Purpose**
TCP 22 Used for administering the operating system.
TCP 80 Port for requests sent over HTTP. Splunk Phantom redirects all HTTP
requests to HTTPS.
TCP 443 HTTPS port for the web interface and REST API.
This port must be exposed to access Splunk Phantom services.

```
Page 8 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### Splunk Phantom cluster node

In a Splunk Phantom cluster, open these ports on each node of the cluster.
**Port Purpose**
TCP 22 Used for administering the Operating System.

```
TCP 80 Port for requests sent over HTTP. Splunk Phantom redirects all HTTP
requests to HTTPS.
TCP 443 HTTPS interface for the web interface and REST API.
This port must be exposed to access Splunk Phantom services.
TCP 4369 RabbitMQ / Erlang port mapper. All cluster nodes must be able to
communicate with each other on this port.
TCP 5671 RabbitMQ service. All cluster nodes must be able to communicate with
each other on this port.
TCP 8300 Consul RPC services. All cluster nodes must be able to communicate
with each other on this port.
TCP 8301 Consul internode communication. All cluster nodes must be able to
communicate with each other on this port.
TCP 8302 Consul internode communication. All cluster nodes must be able to
communicate with each other on this port.
TCP 8888 WebSocket server.
```
```
TCP 15672 RabbitMQ admin UI and HTTP API service. UI is disabled by default. All
cluster nodes must be able to communicate with each other on this port.
TCP 25672 RabbitMQ internode communications. All cluster nodes must be able to
communicate with each other on this port.
```
#### Shared services for clustered environments

Clustered environments require each of these services. Because all these services are
contained in a standalone installation, none of these ports need to be opened.

If you are running Splunk Phantom with external services, such as the PostgreSQL database,
file shares, or Splunk Enterprise, you must open the required ports on both the service’s server
and Splunk Phantom.

```
Page 9 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### All Splunk Phantom nodes

Open these ports on each Splunk Phantom node in a cluster.
**Port Purpose**
TCP 22 Used for administering the Operating System.
SSHD for GlusterFS in clustered environments.
TCP 80 Port for requests sent over HTTP. Splunk Phantom redirects all HTTP
requests to HTTPS.
TCP 443 HTTPS and REST port for HAProxy load balancer for Splunk Phantom.
This port must be exposed to access Splunk Phantom services.
TCP 5100 -
TCP 5120

```
Daemon IPC ports.
```
##### Internode communications

Open these ports on each Splunk Phantom node in a cluster for internode communication.
**Port Purpose**
TCP 4369 RabbitMQ / Erlang port mapper. All cluster nodes must be able to
communicate with each other on this port.
TCP 5671 RabbitMQ service. All cluster nodes must be able to communicate with
each other on this port.
TCP 8300 Consul RPC services. All cluster nodes must be able to communicate
with each other on this port.
TCP 8301 Consul internode communication. All cluster nodes must be able to
communicate with each other on this port.
TCP 8302 Consul internode communication. All cluster nodes must be able to
communicate with each other on this port.
TCP 15672 RabbitMQ admin UI and HTTP API service. UI is disabled by default. All
cluster nodes must be able to communicate with each other on this port.
TCP 25672 RabbitMQ internode communications. All cluster nodes must be able to
communicate with each other on this port.

```
Page 10 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### File share

Open these ports on each Splunk Phantom node and on each member of the GlusterFS server
cluster.
**Port Purpose**
TCP 445 CIFS protocol
UDP 111 RPC portmapper service for GlusterFS and NFS
TCP 111 RPC portmapper service for GlusterFS and NFS
TCP 2049 GlusterFS and NFS for NFS exports. Used by the ​nfsd​ process.
TCP 38465 NFS mount protocol
TCP 38466 NFS mount protocol
TCP 38468 NFS Lock Manager, NLM
TCP 38469 NFS ACL support
TCP ​ 24007 glusterd​ management port
TCP ​ 24008 glusterd​ management port
TCP 49152+ For GlusterFS brick mounts. The total number of ports required to be
open depends on the total number of bricks exported on the server. In
the 4.6 release 10 bricks is sufficient. You might need to open additional
ports later if you add additional bricks.

##### Embedded Splunk Enterprise

These ports must be open on each Splunk Phantom node. If Splunk Enterprise is being hosted
on another server or servers, these ports must be open on those servers as well.
**Port Purpose**
TCP 5121 Splunk Enterprise server HEC service. Can be blocked on the Shared
Services node if using an alternate Splunk Enterprise server.
TCP 5122 Splunk Enterprise server REST port. Can be blocked on the Shared
Services node if using an alternate Splunk Enterprise server.

```
Page 11 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### PostgreSQL

These ports must be open on each Splunk Phantom node and any hosts running the
PostgreSQL service.
**Port Purpose**
TCP ​ 5432 PostgreSQL Service. Can be blocked on the Shared Services node if
using an alternate database server.
TCP 6432 Used by PgBouncer to interact with PostgreSQL database.

#### Mobile Device Registration

These ports must be open on each Splunk Phantom node to enable mobile app registration.
**Port Purpose**
TCP ​ 15505 When the Enable Mobile App toggle is in the ON position,​ ProxyD
connects to the Spacebridge / Automation Broker automatically at
grpc.prod1-cloudgateway.spl.mobi​ to send the interprocess
communication from Phantom to the proxy.
TCP 443 Spacebridge communication port.

```
Others See ​Prerequisites​ in the Install and Administer Splunk Cloud Gateway
guide.
```
```
Page 12 of 84
© 2019 Splunk Inc. All rights reserved.
```

## Install Splunk Phantom using the Amazon Marketplace Image

Install Splunk Phantom for AWS from the AWS Marketplace in the security category.

### Prerequisites

Your AWS instances must meet or exceed the requirements for either an evaluation system for
evaluation or Proof of Value testing, or a production system for production use, and must
include:
● A supported operating system
● Sufficient storage

See ​System requirements​.

**Note:** ​ You need to use a VPN to connect your organization’s infrastructure to an AMI installation
of Splunk Phantom. Instructions for that are outside the scope of this document.

### Installation

1. Log in to your AWS EC2 account.
2. From your EC2 dashboard, select ​ **Launch Instance** ​.
3. In the AWS Marketplace, search for Splunk Phantom.
4. One the Amazon Machine Image entry, click the button ​ **Select** ​.
5. Click ​ **Continue** ​.
6. Select an instance size. The default is ​ **m5.xlarge** ​. Splunk Phantom does not support
    using instances smaller than ​ **t2.xlarge** ​.
7. Click ​ **Next: Configure Instance Details** ​.
8. Configure the instance according to your organization’s policies.
9. Click ​ **Next: Add Storage** ​.

```
Page 13 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
10.Add storage.
```
```
Note ​: You can increase disk size later, but you cannot decrease disk size.
```
```
11.Click ​ Next: Add Tags ​.
```
```
12.Add tags to help identify your Splunk Phantom installation in your EC2 dashboard.
```
```
13.Click ​ Next: Configure Security Group ​.
```
```
14.Configure Security Groups. By default, SSH, HTTP, and HTTPS are permitted from all
IP addresses. Increase security by limiting access to your organization's IP addresses.
```
```
15.Click ​ Review and Launch ​.
```
```
16.Generate or choose SSH keys.
```
```
17.Click ​ Launch Instances ​. The installation typically takes 15 minutes to complete.
```
**Note** ​: In order to log in to the operating system of your AMI-based Splunk Phantom install using
SSH, use the user id ​centos​, not ​root​ or ​ec2-user​. If you need ​root​ access, login as the
centos​ user, then use ​sudo su​.

### Log in to the Splunk Phantom web interface

Connect to the web interface of your newly installed Splunk Phantom instance.

1. Get the public IP address for the instance from the EC2 Management Console.
2. Get the full AWS instance ID for the EC2 instance.
3. Using a browser, go to the public IP address.
    a. User name: ​admin
    b. Password: ​<full AWS instance ID>
4. Change the admin user’s password:
    a. From the ​ **User Name** ​ menu, select ​ **Account Settings.**
    b. From the second level of the menu bar, select ​ **Change Password** ​.
    c. Type the current password.
    d. Type a new password.
    e. Type a new password a second time to confirm.
    f. Click ​ **Change Password** ​.

```
Page 14 of 84
© 2019 Splunk Inc. All rights reserved.
```

## Install Splunk Phantom as a virtual appliance

Splunk Phantom is delivered as a virtual machine image in .OVA format.

Download the virtual appliance image from the Splunk Phantom Community site on the
Products page.

For evaluation or test environments, use a hypervisor or virtual machine management
application such as VMware Fusion®, VMware Fusion Pro®, VMware Workstation Player®,
VMware Workstation Pro®, or Oracle® VirtualBox.

For production environments, use VMware ESXi™ or VMware vSphere® version 5 or higher.

### Install Splunk Phantom with VMware vSphere ESXi or VMware vSphere

These instructions might not be an exact match for the way your VMware vSphere or ESXi
products configured. Consult your vSphere administrator or the documentation on the VMware
website for more options.

**Note:** ​ You can use thin provisioning and install VMware Tools with Splunk Phantom.

1. Log in to the correct vSphere or vCenter asset.
2. From the ​ **File** ​ menu, select ​ **Deploy OVF Template ...**
3. Click ​ **Browse** ​ to locate the downloaded OVA file.
4. Click ​ **Next** ​.
5. Fill out the remaining settings options. Consult the VMware documentation on the
    VMware website or your VMware administrator.
6. Click ​ **Finish** ​.

```
Page 15 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Install Splunk Phantom with VMware Fusion® or VMware Fusion Pro®

For more detailed information on installing virtual machine images, consult the VMware Fusion
or VMware Fusion Pro documentation.

1. Open VMware Fusion or VMware Fusion Pro.
2. From the ​ **File** ​ menu, select ​ **New** ​.
3. Click ​ **More options...**
4. Click ​ **Import an existing virtual machine** ​.
5. Click ​ **Choose File** ​. Navigate to the Splunk Phantom OVA file.
6. Click ​ **Open** ​.
7. Follow the remaining prompts to launch the virtual appliance.

### Install Splunk Phantom with VMware Workstation Pro®

For more detailed information on installing virtual machine images, consult the VMware
Workstation Pro documentation.

1. Open VMware Workstation Pro.
2. Click ​ **Open a Virtual Machine** ​.
3. Navigate to the Splunk Phantom OVA file.
4. Click ​ **Open** ​.
5. Type a name and storage path for the virtual appliance.
6. Click ​ **Import** ​.
7. Click ​ **Power on this virtual machine** ​.

```
Page 16 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Install Splunk Phantom with VMware Workstation Player®

For more detailed information on installing virtual machine images, consult the VMware
Workstation Player documentation.

1. Open VMware Workstation Player.
2. Click ​ **Open a Virtual Machine** ​.
3. Navigate to the Splunk Phantom OVA file.
4. Click ​ **Open** ​.
5. Type a name and storage path for the virtual appliance.
6. Click ​ **Import** ​.
7. Click ​ **Play virtual machine** ​.

```
Note: ​ If you are prompted to connect additional devices, such as sound cards or USB
ports to the virtual machine, decline. These devices are not required to run Splunk
Phantom.
```
### Install Splunk Phantom with Oracle® VirtualBox

For more detailed information on using Oracle VirtualBox to run virtual machine images, consult
the VirtualBox end-user documentation on VirtualBox.org.

1. Start Oracle VirtualBox.
2. From the ​ **File** ​ menu, select ​ **Import Appliance** ​.
3. Select the folder icon to navigate to the Splunk Phantom OVA.
4. Click ​ **Open** ​.
5. Click ​ **Continue** ​.
6. Click ​ **base_vm_centos_7** ​.

```
Page 17 of 84
© 2019 Splunk Inc. All rights reserved.
```

7. Click ​ **Start** ​.

### Complete the Splunk Phantom OVA install

These steps must be completed after the Splunk Phantom virtual appliance has been installed
in your virtual machine manager.

At first boot, Splunk Phantom does several actions automatically:
● Display a splash screen.
● Generate a self-signed SSL certificate.
● Prompt you to set the root and user account password.
● Display a configuration menu.

#### Set operating system passwords

You must set a password. This password is set for both the virtual appliance’s operating system
user accounts ​root​ and ​user​.

Remote SSH is disabled for the ​root​ user. The account ​user​ has ​sudo​ permissions. Use that
account to administer the operating system.

1. Type a password. Save this password somewhere safe.
2. Type the password a second time to confirm it.

Once the password has been set, the configuration menu appears.

#### Assign an IP address to the virtual appliance

The Splunk Phantom virtual appliance is a server. It requires a static IP address for production
environments. You can use DHCP for a test environment.

You can navigate the menu using keyboard arrow keys or the tab key to cycle through options.

1. From the menu, select ​ **Configure Network** ​.
2. Click ​ **OK** ​.
3. Select ​ **Static** ​ to configure a static IP address for Splunk Phantom.
4. Click ​ **OK** ​.

```
Page 18 of 84
© 2019 Splunk Inc. All rights reserved.
```

5. Type the IP address, netmask, gateway, nameserver 1, and nameserver 2.
6. Click ​ **OK** ​.

#### Log in to the Splunk Phantom web interface

Once the IP address is configured the Splunk Phantom web interface is available. You can
configure user accounts and other settings there.

1. Using a web browser, go to the IP address you assigned to Splunk Phantom.
2. Log in using the default credentials.
    ● Username: ​admin
    ● Password: ​password
3. Change the admin user’s password:
    a. From the ​ **User Name** ​ menu, select ​ **Account Settings.**
    b. From the second level of the menu bar, select ​ **Change Password** ​.
    c. Type the current password.
    d. Type a new password.
    e. Type a new password a second time to confirm.
    f. Click ​ **Change Password** ​.

```
Page 19 of 84
© 2019 Splunk Inc. All rights reserved.
```

## Install Splunk Phantom using RPM

This method can be used to install on local hardware or a cloud service, such as AWS or Azure.
Use your Splunk Phantom Community credentials during the installation.

### Prerequisites

Contact Phantom support to get the specific installation package you need.

#### Add the required additional YUM repositories

Add the YUM repositories for your operating system:

```
Operating System Repositories
```
```
CentOS 6.10 ● os
● updates
```
```
CentOS 7.6 ● os
● updates
```
```
Red Hat Enterprise Linux
6.
```
```
● rhel-6-server-rpms
● rhel-6-server-optional-rpms
● rhel-server-rhscl-6-rpms
```
```
Red Hat Enterprise Linux 7.6 ● rhel-7-server-rpms
● rhel-7-server-optional-rpms
● rhel-server-rhscl-7-rpms
```
```
Red Hat Enterprise Linux
on AWS
```
```
● rhui-<region>-rhel-server-optional
```
**Note:** ​ AWS users must edit the file ​/etc/yum.repos.d/redhat-rhui.repo​ on their AWS instance to
enable the additional Red Hat Enterprise Linux repository.

#### Update the operating system and dependencies

Do these actions either as the ​root​ user or a user with ​sudo​ permissions.

1. Clear YUM’s caches.

```
yum clean all
```
2. Update the operating system and all installed packages.

```
Page 20 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
yum update
```
3. Restart the operating system.

```
shutdown -r now
```
### Install Splunk Phantom

1. As either ​root​ or a user with ​sudo​ access, run ​rpm​ to add the required repositories,
    GPG keys, and the installation script.

```
CentOS or RHEL version 6:
https://repo.phantom.us/phantom/4.6/base/6/x86_64/phantom_repo-
4.6.19142-1.x86_64.rpm
```
```
CentOS or RHEL version 7:
https://repo.phantom.us/phantom/4.6/base/7/x86_64/phantom_repo-
4.6.19142-1.x86_64.rpm
```
2. As either ​root​ or a user with ​sudo​ access, run the installation script.

```
/opt/phantom/bin/phantom_setup.sh <option>
```
```
Note: ​ The script prompts you for a username and password. Use your Splunk Phantom
Community credentials.
```
```
For more installation command line options, see the section ​phantom_setup.sh options​.
```
### Log in to the Splunk Phantom web interface

Once the setup script is completed, the Splunk Phantom web interface is available. You can
configure user accounts and other settings there.

1. Using a web browser, go to the IP address you assigned to Splunk Phantom.
2. Log in using the default credentials.
    a. Username: ​admin
    b. Password: ​password
3. Change the admin user’s password:
    a. From the ​ **User Name** ​ menu, select ​ **Account Settings.**

```
Page 21 of 84
© 2019 Splunk Inc. All rights reserved.
```

b. From the second level of the menu bar, select ​ **Change Password** ​.
c. Type the current password.
d. Type a new password.
e. Type a new password a second time to confirm.
f. Click ​ **Change Password** ​.

```
Page 22 of 84
© 2019 Splunk Inc. All rights reserved.
```

## Install Splunk Phantom on a system with limited internet access

Tar file distributions of Splunk Phantom are available for use in offline installations.

### Prerequisites

Contact Phantom support to get this installation file.

Supported operating systems for this method:
● Red Hat Enterprise Linux 6.9
● Red Hat Enterprise Linux 7.5
● CentOS 6.9
● CentOS 7.5

On Red Hat Enterprise Linux, you must either create a satellite server or local YUM repository
for operating system packages and other dependencies. See the Red Hat Knowledgebase
article, “How can we regularly update a disconnected system (A system without internet
connection)?”.

#### Update the operating system and dependencies

Do these tasks with ​root​ permissions, either by logging in as ​root​ or as a user with ​sudo
permission.

1. Clear YUM caches.

```
yum clean all
```
2. Update installed packages.

```
yum update
```
3. Restart the operating system.

```
shutdown -r now
```
```
Page 23 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Install Splunk Phantom from the tar file

1. Make a directory for the tar file.

```
mkdir /usr/local/src/upgrade-<version>
```
2. Change to the created directory.

```
cd /usr/local/src/upgrade-<version>
```
3. Download or copy the tar file to the directory.
4. Extract the tar file.

```
tar -xvzf phantom_offline_setup_<OS>-<version>.tgz
```
5. Change to the directory phantom_offline_setup.

```
cd phantom_offline_setup_<OS>-<version>
```
6. Run the installation script.

```
./phantom_offline_setup_<OS>.sh install
```
```
If you want to install without apps, add ​--without-apps​ to the command.
```
```
For more installation command line options, see “phantom_offline_setup_<OS>.sh
options.”
```
### Log in to the Splunk Phantom web interface

Once the setup script is completed, the Splunk Phantom web interface is available. You can
configure user accounts and other settings there.

1. Using a web browser, go to the IP address you assigned to Splunk Phantom.
2. Log in using the default credentials.
    ● Username: ​admin
    ● Password: ​password

```
Page 24 of 84
© 2019 Splunk Inc. All rights reserved.
```

3. Change the admin user’s password
    a. From the ​ **User Name** ​ menu, select ​ **Account Settings.**
    b. From the second level of the menu bar, select ​ **Change Password** ​.
    c. Type the current password.
    d. Type a new password.
    e. Type a new password a second time to confirm.
    f. Click ​ **Change Password** ​.

## Install Splunk Phantom as an unprivileged user

Tar file distributions of Splunk Phantom are available for installations where Splunk Phantom
will run as an unprivileged user.

**Note** ​: If you install a stand-alone Splunk Phantom instance as an unprivileged user, underlying
services such as the PostgreSQL database are installed in the user space for that user.

### Prerequisites

Contact Phantom support to get this installation file.

Supported Operating Systems for this method:
● Red Hat Enterprise Linux 7.6
● CentOS 7.6

### Prepare the system

Before you install Splunk Phantom as an unprivileged user, the ​root​ user or a user with ​sudo
access must prepare the system.

Do all these tasks with ​root​ permissions, either by logging in as ​root​ or as a user with ​sudo
permission.

#### Install the operating system dependencies

1. Edit ​/etc/selinux/config​ to disable SELinux. Change the ​SELINUX=​ entry to
    SELINUX=disabled​.
2. Clear yum caches.

```
yum clean all
```
```
Page 25 of 84
© 2019 Splunk Inc. All rights reserved.
```

3. Update installed packages.

```
yum update
```
4. Restart the operating system.

```
shutdown -r now
```
5. Install dependencies.

```
yum install -y libevent c-ares bind-utils
java-1.8.0-openjdk-headless mailcap fontconfig ntpdate perl
rsync xmlsec1 xmlsec1-openssl libxslt ntp zip net-tools
policycoreutils-python libxml2 libcurl gnutls
```
6. If you are using an external file share using GlusterFS, download the GlusterFS
    packages.

```
mkdir gfinstall
cd gfsinstall
```
```
curl -O
https://repo.phantom.us/phantom/4.6/base/7/x86_64/glusterfs-4.1
.6-1.el7.x86_64.rpm
curl -O
https://repo.phantom.us/phantom/4.6/base/7/x86_64/glusterfs-lib
s-4.1.6-1.el7.x86_64.rpm
curl -O
https://repo.phantom.us/phantom/4.6/base/7/x86_64/glusterfs-cli
-4.1.6-1.el7.x86_64.rpm
curl -O
https://repo.phantom.us/phantom/4.6/base/7/x86_64/glusterfs-fus
e-4.1.6-1.el7.x86_64.rpm
curl -O
https://repo.phantom.us/phantom/4.6/base/7/x86_64/glusterfs-cli
ent-xlators-4.1.6-1.el7.x86_64.rpm
```
7. If you are using an external file share using GlusterFS, install the GlusterFS packages.

```
yum install *.rpm
```
```
Page 26 of 84
© 2019 Splunk Inc. All rights reserved.
```

8. Set firewall rules to allow the required ports listed in “Splunk Phantom required ports.”
9. Synchronize the system clock.

```
ntpdate -v -u 0.centos.pool.ntp.org
systemctl enable ntpd
```
10.Create a file called ​/etc/sysctl.d/50-phantom.conf​. Use this file to supply
kernel settings required by Splunk Phantom.

```
touch /etc/sysctl.d/50-phantom.conf
```
11.Edit the file ​/etc/sysctl.d/50-phantom.conf ​to add these settings.

```
vi /etc/sysctl.d/50-phantom.conf
```
```
# Turn off IP packet forwarding
net.ipv4.ip_forward = 0
```
```
# Turn on source route verification
net.ipv4.conf.default.rp_filter = 1
```
```
# Do not accept source routing
net.ipv4.conf.default.accept_source_route = 0
```
```
# Controls the System Request debugging functionality
# of the kernel
kernel.sysrq = 0
```
```
# Do not send redirects (This system is not a router.)
net.ipv4.conf.all.send_redirects=0
net.ipv4.conf.default.send_redirects=0
```
```
# Do not accept IP source routing
net.ipv4.conf.all.accept_source_route=0
```
```
# Do not accept redirects, secure or not
net.ipv4.conf.all.accept_redirects=0
net.ipv4.conf.default.accept_redirects=0
net.ipv4.conf.all.secure_redirects=0
net.ipv4.conf.default.secure_redirects=0
```
```
Page 27 of 84
© 2019 Splunk Inc. All rights reserved.
```

# Log any unexpected packets
net.ipv4.conf.all.log_martians=1
net.ipv4.conf.default.log_martians=1

# Filter ICMP broadcast or bogus ones
net.ipv4.icmp_echo_ignore_broadcasts=1
net.ipv4.icmp_ignore_bogus_error_responses=1

# Enable reverse path checking
net.ipv4.conf.all.rp_filter=1
net.ipv4.conf.default.rp_filter=1
net.ipv6.conf.all.accept_ra=0
net.ipv6.conf.default.accept_ra=0
net.ipv6.conf.all.accept_redirects=0
net.ipv6.conf.default.accept_redirects=0
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1

# Controls whether core dumps will append the
# PID to the core filename.
# Useful for debugging multi-threaded applications.

### If directed by Splunk Phantom Support, uncomment these
lines,
### replace <username> with the user name that runs Splunk
### Phantom, then run "sysctl -p" to enable core files.
# kernel.core_uses_pid = 1
# kernel.core_pattern =
/home/<username>/phantom/.cores/core-%e-%s-%u-%g-%p-%t
# fs.suid_dumpable = 2

# Controls the use of TCP SYN cookies
net.ipv4.tcp_syncookies = 1

# Controls the default maximum size of a message queue
kernel.msgmnb = 65536

# Controls the maximum size of a message, in bytes
kernel.msgmax = 65536

```
Page 28 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
# Controls the maximum shared segment size, in bytes
kernel.shmmax = 68719476736
```
```
# Controls the maximum number of shared memory segments, in
pages
kernel.shmall = 4294967296
```
```
12.Save the file.
```
```
13.Apply the new kernel settings.
```
```
sysctl --system
```
#### Create the user account that will run Splunk Phantom

An unprivileged install of Splunk Phantom will run in the user space of a specific user.

**Caution:** ​ When you create the user account that will run Splunk Phantom, do not use special
characters other than a hyphen or underscore. A hyphen or underscore must not be the first
character in the username. Other special characters are not supported.

1. Create the user account that will be used to run Splunk Phantom.

```
adduser -c "Phantom User" <username>
passwd <username>
```
2. Create a directory for Splunk Phantom.

```
su - <username> -c "mkdir /home/<username>/<directory_name>"
```
3. Create a file called ​/etc/security/limits.d/25-phantom-limits.conf​. This
    file sets resource limits for the user that will run Splunk Phantom.

```
touch /etc/security/limits.d/25-phantom-limits.conf
```
4. Edit the file ​/etc/security/limits.d/25-phantom-limits.conf ​to add these
    settings.

```
vi /etc/security/limits.d/25-phantom-limits.conf
```
```
<username> hard nofile 64000
<username> soft nofile 64000
Page 29 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
<username> hard nproc 64000
<username> soft nproc 64000
```
5. Save the file.
6. Apply the new security settings.

```
sysctl --system
```
### Install Splunk Phantom from the tar file

1. Log in as the user that will run Splunk Phantom.
2. Copy the installation tar file to the directory
    /home/<username>/<directory_name> ​created earlier.
3. Verify your downloaded file’s SHA256 hash against the SHA256 hash displayed on the
    Splunk Phantom Community site.

```
sha256sum phantom-<version>.tgz
```
4. Extract the Splunk Phantom tar file.

```
tar -xvzf phantom-<version>.tgz
```
5. Run the installation script. Make sure to specify the ports for Splunk Phantom to use for
    HTTPS traffic. The HTTPS port cannot be a port which is already in use. This port must
    be a port greater than 1023.

```
./phantom_tar_install.sh install --https-port=<port>
```
```
For more installation command line options, see ​phantom_tar_install.sh options​.
```
### Log in to the Splunk Phantom web interface

1. Log in to Splunk Phantom’s web interface at the custom https port.
    a. https://<ip address or hostname>:<your https port>
    b. Username: ​admin
    c. Password: ​password

```
Page 30 of 84
© 2019 Splunk Inc. All rights reserved.
```

2. Change the admin user’s password:
    a. From the ​ **User Name** ​ menu, select ​ **Account Settings.**
    b. From the second level of the menu bar, select ​ **Change Password** ​.
    c. Type the current password.
    d. Type a new password.
    e. Type a new password a second time to confirm.
    f. Click ​ **Change Password** ​.

## Create a Splunk Phantom cluster

Splunk Phantom supports clustering. Clusters of multiple Splunk Phantom installations share
the services of a PostgreSQL database, file shares, Splunk Enterprise, and use an HAProxy​®
server ​as a load balancer.

A cluster consists of a minimum of three (3) instances of Splunk Phantom, and any external
services that support them. The supporting services are file shares, a PostgreSQL database,
Splunk Enterprise, and at least one load balancer, such as HAProxy.

Before creating a cluster, work with your Splunk Phantom Delivery Team representative to
assess your needs and design your cluster.

Clusters can be built in several ways.
● From virtual appliances converted to a server or cluster node. You convert a virtual
appliance to a specific role using provided scripts.
● From privileged installations, where required services are provided by servers external to
Splunk Phantom. Each Splunk Phantom node is converted from an rpm or tar file
installation using the ​make_cluster_node.pyc​ script.
● From unprivileged installations, where required services are provided by servers external
to Splunk Phantom. Each Splunk Phantom node is converted from an rpm or tar file
installation using the ​make_cluster_node.pyc​ script.
● From Amazon Marketplace Images, where required services are provided by AWS
components such as Elastic Load Balancer (ELB), Elastic File System (EFS), and
Relational Database System (RDS). Splunk Phantom instances are converted to cluster
nodes and a Splunk Enterprise Instance by using the ​prepare_db.py​,
make_server_node.pyc​, and ​make_cluster_node.pyc​ scripts.

### Create a Splunk Phantom Cluster from OVA installs

**Caution:** ​ Converting a Splunk Phantom virtual appliance to a server or cluster node is a
one-way operation. It cannot be reverted.

```
Page 31 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### Build a cluster with a single Shared Services node

The most basic version of a Splunk Phantom cluster is a single Shared Services node
connected to multiple instances of Splunk Phantom.

**Caution:** ​ This configuration is not recommended for production use. This mode is primarily
intended for Proof of Value or demonstrations. A single Shared Services node becomes a
single point of failure. Any problems on the Shared Services node impact your entire Splunk
Phantom cluster.

##### Checklist - Single Shared Services node

```
Number Task Description Documentation
```
```
1 Create the Shared
Services Node.
```
1. Install Splunk Phantom as
    a virtual appliance.
2. Run the
    make_server_node
    script to build your Shared
    Services node.

```
Install Splunk Phantom as a
virtual appliance
```
```
make_server_node.pyc
```
```
2 Install Splunk
Phantom cluster
nodes.
```
1. Install Splunk Phantom as
    a virtual appliance, once
    for each node you need in
    your cluster.
2. Make the first cluster
    node.
    make_cluster_node.py
    c.
3. Make additional cluster
    nodes.

```
Install Splunk Phantom as a
virtual appliance
```
```
make_cluster_node.pyc
```
#### Build a cluster with external service services

Build a more robust cluster, putting each of the services on its own server or group of servers to
serve multiple cluster nodes of Splunk Phantom.

**Caution:** ​ Converting a Splunk Phantom virtual appliance to a server or cluster node is a
one-way operation. It cannot be reverted.

```
Page 32 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### Checklist - virtual machine images cluster with external services

```
Number Task Description Documentation
```
```
1 Create the
HAProxy node
```
1. Install Splunk Phantom as
    a virtual appliance.
2. Run
    make_server_node
    install proxy

```
Install Splunk Phantom as a
virtual appliance
```
```
make_server_node.pyc
```
```
2 Create the
PostgreSQL node
```
1. Install Splunk Phantom as
    a virtual appliance.
2. Run
    make_server_node
    install db

```
Install Splunk Phantom as a
virtual appliance
```
```
make_server_node.pyc
```
```
3 Create the file
shares node
```
1. Install Splunk Phantom as
    a virtual appliance.
2. Run
    make_server_node
    install fs

```
Install Splunk Phantom as a
virtual appliance
```
```
make_server_node.pyc
```
```
4 Create the Splunk
Enterprise node
```
1. Install Splunk Phantom as
    a virtual appliance.
2. Run
    make_server_node
    install splunk

```
Install Splunk Phantom as a
virtual appliance
```
```
make_server_node.pyc
```
```
5 Install Splunk
Phantom cluster
nodes
```
1. Install Splunk Phantom as
    a virtual appliance, once
    for each node you need in
    your cluster.
2. Make the first cluster
    node.
    make_cluster_node.py
    c.
3. Make additional cluster
    nodes.

```
Install Splunk Phantom as a
virtual appliance
```
```
make_cluster_node.pyc
```
```
Page 33 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Create a Splunk Phantom Cluster from rpm or tar file installs

Build a cluster, putting each of the services on its own server or group of servers to serve
multiple cluster nodes of Splunk Phantom.

**Caution:** ​ Converting a Splunk Phantom installation to a server or cluster node is a one-way
operation. It cannot be reverted.

##### Checklist - cluster with external services

```
Number Task Description Documentation
1 Create the
HAProxy server
```
```
Use the HAProxy server to be a
load balancer for the Splunk
Phantom nodes in your cluster.
```
```
Set up an HAProxy server
```
```
2 Create the
PostgreSQL server
or cluster
```
```
Establish a PostgreSQL database
server or cluster to store Splunk
Phantom information.
```
```
Set up the external PostgreSQL
server
```
```
3 Create the file
shares server
```
```
Splunk Phantom will store all its
shared files on the prepared
GlusterFS server.
```
```
You can use NFS or other network
file system. Instructions for that are
not included in this document.
```
```
Set up the external file shares
```
```
4 Install Splunk
Enterprise
```
```
Splunk Phantom will use Splunk
Enterprise for searches and collect
data for indexing using the HTTP
Event Collector.
```
```
Set up Splunk Enterprise
```
```
5 Install Splunk
Phantom cluster
nodes
```
1. Install Splunk Phantom
    using the rpm for
    privileged installs or tar file
    method for unprivileged
    installs. Do this once for
    each node you need in
    your cluster.
2. Make the first node with
    make_cluster_node.py
    c.
3. Make additional nodes.

```
Install Splunk Phantom using
rpm
```
```
Install Splunk Phantom as an
unprivileged user
```
```
make_cluster_node.pyc
```
```
Page 34 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Create a Splunk Phantom cluster using unprivileged installs

Build a cluster, putting each of the services on its own server or group of servers to serve
multiple cluster nodes of Splunk Phantom.

Set up each of the external services either as the ​root​ user or a user with ​sudo​ permissions.

Install Splunk Phantom as an unprivileged user. In your cluster, each Splunk Phantom instance
must have the same custom username and install directory. See ​Install Splunk Phantom as an
unprivileged user​.

**Caution:** ​ Converting a Splunk Phantom installation to a cluster node is a one-way operation. It
cannot be reverted.

##### Checklist - unprivileged install cluster with external services

```
Number Task Description Documentation
1 Create the
HAProxy node
```
```
Use the HAProxy server to be a
load balancer for the Splunk
Phantom nodes in your cluster.
```
```
Set up an HAProxy server
```
```
Warning ​: There are
additional steps to configure
your load balancer to handle
your custom HTTPS PORT
for unprivileged clusters.
```
```
2 Create the
PostgreSQL node
```
```
Establish a PostgreSQL database
server or cluster to store Splunk
Phantom information.
```
```
Set up the external PostgreSQL
server
```
```
3 Create the file
shares node
```
```
Splunk Phantom will store all its
shared files on the prepared
GlusterFS server.
```
```
You can use NFS or other network
file system. Instructions for that are
not included in this document.
```
```
Set up the external file shares
```
```
4 Create the Splunk
Enterprise node
```
```
Splunk Phantom will use Splunk
Enterprise for searches and collect
data for indexing using the HTTP
Event Collector.
```
```
Set up Splunk Enterprise
```
```
5 Install Splunk
Phantom cluster
nodes
```
1. Install Splunk Phantom
    using the tar file method
    for unprivileged installs.
    Do this once for each node

```
Install Splunk Phantom as an
unprivileged user
```
```
make_cluster_node.pyc
```
```
Page 35 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
you need in your cluster.
```
2. Make the first node with
    make_cluster_node.py
    c.
3. Make additional nodes.

### Run make_server_node.pyc

Use the ​make_server_node.pyc​ script to convert a virtual machine image install into either a
specific service or a Shared Services node for a Splunk Phantom cluster.

**Caution:** ​ Converting a Splunk Phantom installation to a server node is a one-way operation. It
cannot be reverted.

#### Additional configuration steps for unprivileged clusters

Perform the following steps on the load balancer or Shared Services Node as ​root​ or as a user
using ​sudo​ to get elevated permissions.

1. Set SELINUX to allow HAProxy to bind to your custom HTTPS port. Note: If SELINUX is
    disabled, then proceed to step 2.

```
semanage port --add --type http_port_t --proto tcp <HTTPS PORT>
```
```
If you receive an error that the port is already defined, use ​--modify​ instead of ​--add​.
```
```
semanage port --modify --type http_port_t --proto tcp <HTTPS
PORT>
```
2. Edit ​/etc/haproxy/haproxy.cfg​ to remove the comment marker # from the
    frontend block on the line for your custom HTTPS port.

```
# bind *:<HTTPS PORT> ssl crt /etc/haproxy/ ...
```
```
Becomes:
```
```
bind *:<HTTPS PORT> ssl crt /etc/haproxy/ ...
```
3. Restart HAProxy.

```
Page 36 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
systemctl restart rh-haproxy18-haproxy
```
#### Create a Shared Services node

**Note:** ​ A single Shared Services node is not recommended for production use. This mode is
primarily intended for Proof of Value or demonstrations.

A single Shared Services node becomes a single point of failure. Any problems on the Shared
Services node impact your entire Splunk Phantom cluster. For production use, build a node for
each service rather than a single Shared Services node.

Create a Shared Services node as ​root​ or using ​sudo​:

/opt/phantom/bin/phenv python /opt/phantom/bin/make_server_node.pyc

Making a Shared Services node also generates a file named ​mcn_responses.json​, in
/opt/phantom/bin​. This file can be passed as an argument to ​make_cluster_node.pyc
to help setting up the first Splunk Phantom node in your cluster.

**Note:** ​ The ​mcn_responses.json​ file contains secrets such as usernames and passwords.
Store it someplace safe.

#### Create a specific function node

Create a specific function node, such as an HAProxy load balancer, PostgreSQL database, file
share, or Splunk Enterprise as ​root​ or using ​sudo:

/opt/phantom/bin/phenv python /opt/phantom/bin/
make_server_node.pyc --<option argument>

Repeat once on separate virtual machine image installations for each server node.

**Valid arguments**
● fs​ - sets up a single node GlusterFS for file shares.
● db​ - sets up the internal PostgreSQL database to be used as an external PostgreSQL
database.
● proxy​ - installs and configures HAProxy to serve as a load balancer for your Splunk
Phantom cluster.
● splunk​ - allows the local Splunk Enterprise to be used as a remote search endpoint.

```
Page 37 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### make_sever_node.pyc prompts and warnings

The make_server_node.pyc script issues a warning that you are about to permanently change
your Splunk Phantom instance.

The changes are:
● Splunk Phantom is removed from system boot scripts.
● Disabling the internal Splunk Phantom database.
● Configuring file shares.
● Installing HAProxy to act as a load balancer.
● Installing Splunk Enterprise.
You must respond to the warning with “​y​” for yes to proceed.

You are prompted to supply information for the TLS certificate.
● Country Code
● State Code
● City
● Organization
● Organization unit
● Hostname (or IP address)
● Email address

The remaining prompts are:
● The subnet on which PostgreSQL will accept connections.
● Set the passwords for the ​postgres​ and ​pgbouncer​ user accounts.
● Password for the ​user​ account.

When the script completes it writes the file ​/opt/phantom/bin/mcn_responses.json​.

Logs are written to:
/var/log/phantom/make_server_node/make_server_node_<date and
time>.log

### Run make_cluster_node.pyc

Use the ​make_cluster_node.pyc​ script to configure an installed Splunk Phantom instance
into a node of a cluster. This script stores the bulk of required configuration information the
PostgreSQL database.

Before running ​make_cluster_node​, make sure that all the required services are working,
either as external services or as a Shared Services node.
Page 38 of 84
© 2019 Splunk Inc. All rights reserved.


**Caution:** ​ Converting an installation of Splunk Phantom to a cluster node is a one-way
operation. It cannot be reverted.

#### Collect the required information

You need this information to answer prompts for ​make_cluster_node​.
● IP addresses or hostnames for:
○ PostgreSQL 9.4 server
○ HAProxy server
■ port the HAProxy server uses to accept HTTPS connections
○ GlusterFS server
○ Splunk Enterprise instance
■ REST port
■ HTTP Event Collector port
● User names, passwords, tokens, or SSH key information for:
○ pgbouncer​ PostgreSQL database user
○ postgres​ PostgreSQL database user
○ login password for the HAProxy server, unless it uses an ssh key
○ Splunk Phantom username and password for the install being converted
○ Splunk Enterprise
■ user with ​phantomsearch​ permissions
■ user with ​phantomdelete​ permissions
■ HTTP Event Collector token

#### Create a Splunk Phantom node

Once you have either a Shared Services node or external services established, you convert
installations of Splunk Phantom into cluster nodes.

##### Privileged installation

On a privileged installation, such as a virtual machine image, or an RPM installation, run the
make_cluster_node.pyc​ script as ​root​ or a user with ​sudo​ permissions.

1. Run the ​make_cluster_node.pyc​ script.

```
/opt/phantom/bin/phenv python /opt/phantom/bin/
make_cluster_node.pyc --responses /path/to/mcn_responses.json
```
```
Note: ​ You don’t have to use ​mcn_responses.json​. If you do not supply a JSON file,
the script prompts you for the information it needs.
```
```
Page 39 of 84
© 2019 Splunk Inc. All rights reserved.
```

2. For each other node, run the script without arguments.

```
/opt/phantom/bin/phenv python /opt/phantom/bin/
make_cluster_node.pyc
```
##### Unprivileged installation

On an unprivileged installation you must first change to the directory where Splunk Phantom is
installed.

1. Change to the Splunk Phantom home directory.

```
cd <phantom_install_dir>/bin/
```
2. Run ​make_cluster_node.pyc​ using python 2.7.

```
phenv python2.7 ./make_cluster_node.pyc --responses
/path/to/mcn_responses.json
```
```
Note: ​ You don’t have to use ​mcn_responses.json​. If you do not supply a JSON file,
the script prompts you for the information it needs.
```
### Create a Splunk Phantom Cluster in Amazon Web Services

Build a cluster from AMI-based instances of Splunk Phantom, building several of the required
services using the AWS native components, Elastic Load Balancer (ELB), Elastic File System
(EFS), and Relational Database System (RDS).

This configuration is built using the Amazon Marketplace Image of Splunk Phantom.
Unprivileged installations are not supported.

#### Prerequisites

```
● Open a support case with Phantom Support to obtain a copy of the ​prepare_db.py
script for your version of Splunk Phantom.
```
**Caution:** ​ Converting a Splunk Phantom installation to a server or cluster node is a one-way
operation. It cannot be reverted.

```
Page 40 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### Checklist - AMI-based cluster, using AWS services

```
Number Task Description Documentation
```
```
1 Launch and
prepare AMI
instances of
Splunk Phantom.
```
```
Total number of Splunk Phantom
AMI instances = Number of cluster
nodes + 1
```
```
Launch and prepare AMI
instances of Splunk Phantom.
```
```
2 Create a load
balancer with
Elastic Load
Balancer (ELB).
```
1. Create the ELB.
2. Create the Target Group.
3. Add routing rules.

```
Create a load balancer with
Elastic Load Balancer (ELB).
```
```
3 Create the file
stores with Elastic
File System (EFS).
```
```
Create the EFS file store for
shared files.
```
```
Create the file stores with Elastic
File System (EFS).
```
```
4 Create the external
database with
Relational
Database System
(RDS).
```
1. Create the external
    PostgreSQL database.
2. Create the pgbouncer
    user.

```
Create the external PostgreSQL
database with the Relational
Database System.
```
```
5 Add the file shares
to each Splunk
Phantom instance.
```
```
Mount the file shares on each
Splunk Phantom instance.
```
```
Add the file shares to each
Splunk Phantom instance.
```
```
6 Convert an
AMI-based Splunk
Phantom Instance
into the Splunk
Enterprise
instance.
```
```
Convert one of the Splunk
Phantom instances into the Splunk
Enterprise instance. This instance
will serve as the external search
endpoint for the entire cluster. Use
the ​make_server_node.pyc
script with the ​splunk​ arguments.
```
```
Convert an AMI-based Splunk
Phantom Instance into the
Splunk Enterprise instance.
```
```
7 Convert the first
AMI-based Splunk
Phantom instance
into a cluster node.
```
```
Convert the first Splunk Phantom
instance into a cluster node.
Creating the first node will use a
script option to record all the
make_cluster_node.pyc​ script
answers to a file for using on each
of your other nodes.
```
```
Convert the first AMI-based
Splunk Phantom instance into a
cluster node​.
```
```
8 Convert the
remaining
AMI-based Splunk
Phantom instances
into cluster nodes.
```
```
Convert the remaining Splunk
Phantom instances into cluster
nodes using
make_cluster_node.pyc​ and
the ​responses.json​ file.
```
```
Convert the remaining
AMI-based Splunk Phantom
instances into cluster nodes.
```
```
Page 41 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### Launch and prepare AMI-based instances of Splunk Phantom

You need a number of AMI-based Splunk Phantom instances equal to the number of Splunk
Phantom nodes you want in your cluster plus one. The additional instance will be converted into
the externalized Splunk Enterprise instance for your cluster. A Splunk Phantom cluster requires
a minimum of three nodes.

```
Total number of Splunk Phantom AMI instances = Number of cluster nodes + 1
```
If you already have a Splunk Enterprise deployment that you will use instead, follow the
instructions for using an external Splunk Enterprise instance found in the chapter ​Run Splunk
Phantom using external services​.

#### Installation

1. Log in to your AWS EC2 account.
2. From your EC2 dashboard, select ​ **Launch Instance** ​.
3. In the AWS Marketplace, search for Splunk Phantom.
4. One the Amazon Machine Image entry, click the button ​ **Select** ​.
5. Click ​ **Continue** ​.
6. Select an instance size. The default is ​ **m5.xlarge** ​. Splunk Phantom does not support
    using instances smaller than ​ **t2.xlarge** ​.
7. Click ​ **Next: Configure Instance Details** ​.
8. For ​ **Number of Instances** ​, type the number of instances you need.

```
Total number of Splunk Phantom AMI instances = Number of cluster nodes + 1
```
9. Configure the instances according to your organization’s policies.

```
10.Click ​ Next: Add Storage ​.
```
```
11.Add storage.
```
```
Page 42 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
Note ​: You can increase disk size later, but you cannot decrease disk size.
```
```
12.Click ​ Next: Add Tags ​.
```
```
13.Add tags to help identify your Splunk Phantom installation in your EC2 dashboard.
```
```
14.Click ​ Next: Configure Security Group ​.
```
```
15.Configure Security Groups. By default, SSH, HTTP, and HTTPS are permitted from all
IP addresses. Increase security by limiting access to your organization's IP addresses.
```
```
16.Click ​ Review and Launch ​.
```
```
17.Generate or choose SSH keys.
```
```
18.Click ​ Launch Instances ​. The installation typically takes 15 minutes to complete.
```
**Note** ​: In order to log in to the operating system of your AMI-based Splunk Phantom install using
SSH, use the user id ​centos​, not ​root​ or ​ec2-user​. If you need ​root​ access, login as the
centos​ user, then use ​sudo su - ​.

##### Install support for Elastic File System (EFS)

In order to use EFS as your external file stores, the custom version of nfs it uses must be
installed on each AMI-based Splunk Phantom instance.

Do these steps on each AMI-based Splunk Phantom instance.

1. Login to the AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
2. Get the required files to build EFS support.

```
sudo yum -y install git nfs-utils make rpm-build
```
3. Get the git repository that contains the source for EFS support.

```
git clone ​https://github.com/aws/efs-utils
```
4. Change directory to the efs-utils directory.

```
cd efs-utils
```
```
Page 43 of 84
© 2019 Splunk Inc. All rights reserved.
```

5. Build the rpm packages from the source files.

```
sudo make rpm
```
6. Install the packages.

```
sudo yum -y install ./build/amazon-efs-utils*rpm
```
7. Return to the ​centos​ user’s home directory.

```
cd -
```
##### Install SSH keys

During the conversion to Splunk Phantom cluster nodes, each instance will need to SSH as the
centos​ user into other nodes. Install the client certificate you generated for SSH when the
instances were created.

Do this on each of the instances that you will convert to cluster nodes.

1. Copy the ​.pem​ file generated earlier to each instance using SCP.

```
scp -i <path/to/.pem> <path/to/.pem to transfer>
centos@<instance IP or DNS name>:~/
```
2. SSH to an AMI-based Splunk Phantom instance as the ​centos​ user.
3. Move the .pem key to the centos user’s ​.ssh​ directory.

```
mv <name of file>.pem .ssh
```
4. Set the permissions on the ​.pem​ key.

```
chmod 600 .ssh/<name of file>.pem
```
5. Test that you are able to SSH from each instance to the others as the ​centos​ user.

##### Replace database preparation scripts

Finally, replace the ​prepare_db.pyc​ file with the ​prepare_db.py​ file you obtained from
Phantom Support.

Do this on each AMI-based Splunk Phantom instance.

```
Page 44 of 84
© 2019 Splunk Inc. All rights reserved.
```

1. SCP the file ​prepare_db.py​ to each AMI-based Splunk Phantom instance.

```
scp -i <path/to/pem-key> <path/to/>prepare_db.py centos@<DNS
name of the instance>:~/
```
2. Login to the AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
3. Elevate to ​root​.

```
sudo su -
```
4. Rename the original ​prepare_db.pyc​ file.

```
sudo mv /opt/phantom/bin/prepare_db.pyc
/opt/phantom/bin/prepare_db.pyc.original
```
5. Copy the new ​prepare_db.py​ file to ​/opt/phantom/bin​.

```
sudo cp /home/centos/prepare_db.py
/opt/phantom/bin/prepare_db.py
```
#### Create a load balancer with Elastic Load Balancer (ELB)

Create a load balancer for your Splunk Phantom cluster. An Elastic Load Balancer will be used
instead of HAProxy.

1. Log in to your AWS EC2 account.
2. From the menu on the EC2 dashboard, under the heading ​ **Load Balancing** ​, choose
    **Load Balancers** ​.
3. Click ​ **Create Load Balancer** ​.
4. Under ​ **Application Load Balancer** ​, click ​ **Create** ​.
5. Type a name for your load balancer in the ​ **Name** ​ field.
6. Select a ​ **Scheme** ​. The scheme will depend on your AWS network configuration.
    Assuming your load balancer will route on an internal network, select the ​ **internal** ​ radio
    button.

```
Page 45 of 84
© 2019 Splunk Inc. All rights reserved.
```

7. Set the IP address type. This will also depend on your AWS network configuration. In
    most cases, select ​ **ipv4** ​ from the menu.
8. Under ​ **Listeners** ​, ​ **Load Balancer Protocol** ​, select ​ **HTTPS** ​ from the menu. The ​ **Load**
    **Balancer Port** ​ changes to 443.
9. Under Availability Zones, select the VPC and Availability Zones to match your AWS
    network configuration.

10.Add ​ **Tags** ​ to help organize and identify your load balancer.

11.Click ​ **Next: Configure Security Settings.**

12.Configure security certificates according to your organization’s policies. These settings
can vary based on factors outside the scope of this document.

13.Click ​ **Next: Configure Security Groups**

14.Select or create a security group according to your organization’s policies. These
settings can vary based on factors outside the scope of this document.

15.Click ​ **Next: Configure Routing.**

16.Under ​ **Target group** ​, choose ​ **New target group** ​ from the menu.

17.Type a name for your target group in the ​ **Name** ​ field.

18.For ​ **Target type** ​, select the ​ **Instance** ​ radio button.

19.For ​ **Protocol** ​, select ​ **HTTPS** ​ from the menu. ​ **Port** ​ changes to ​ **443** ​ automatically.

20.Under ​ **Health checks** ​, set ​ **Protocol** ​ to ​ **HTTPS** ​.

21.In the ​ **Path** ​ field, type ​ **/check** ​.

22.Click ​ **Next: Register Targets.**

23.Under ​ **Instances** ​, find and select the instances for your Splunk Phantom cluster.

24.Click ​ **Add to registered.**

```
Page 46 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
25.Click ​ Next: Review.
```
```
26.Review and correct any errors.
```
```
27.Click ​ Create ​.
```
##### Create a Target Group to be used by your load balancer

This target group will be used to route websockets traffic for the Splunk Phantom Cluster.

1. In the sidebar on the EC2 dashboard, under ​ **Load Balancing** ​, select ​ **Target Groups** ​.
2. Click ​ **Create target group** ​.
3. In the ​ **Create target group** ​ dialog:
    a. Type a name in the ​ **Target group name** ​ field.
    b. Select the ​ **Instance** ​ radio button.
    c. Select ​ **HTTPS** ​ from the ​ **Protocol** ​ menu. ​ **Port** ​ will change to ​ **443** ​.
    d. Select the same ​ **VPC** ​ that your target instances are using from the menu.
    e. Under ​ **Health Check settings** ​, select ​ **HTTPS** ​ from the ​ **Protocol** ​ menu.
    f. In the ​ **Path** ​ field, type ​ **/check** ​.
4. Click ​ **Create** ​.
5. From the target groups list, select the target group you just created.
6. On the ​ **Description** ​ tab, under ​ **Attributes** ​, click ​ **Edit attributes** ​.
7. In the ​ **Edit attributes** ​ dialog:
    a. For ​ **Stickiness** ​, select the ​ **Enable** ​ check box.
    b. Set ​ **Stickiness duration** ​ by typing ​ **7** ​ and choosing ​ **days** ​ from the menu.
    c. Click ​ **Save** ​.

##### Add the routing rules to your load balancer

1. From the EWS menu, under ​ **Load Balancing** ​, select ​ **Load Balancers** ​.
2. Select the load balancer you have created for your Splunk Phantom cluster.
3. Click the ​ **Listeners** ​ tab.
4. Under ​ **Rules** ​, click the ​ **View/edit rules** ​ link.

```
Page 47 of 84
© 2019 Splunk Inc. All rights reserved.
```

5. Click the ​ **+** ​ icon to add a new rule.
6. Click the ​ **+ Insert Rule** ​ link to edit the rule.
7. Under ​ **IF (all match)** ​, click ​ **+ Add condition** ​.
8. Select ​ **Path...** ​, then type ​ **/websocket** ​ in the text box.
9. Click the checkmark icon.

```
10.Under ​ THEN ​, click ​ + Add condition ​.
```
```
11.Select​ Forward to... ​, then select the name of your websockets listener from the menu.
```
```
12.Click the checkmark icon.
```
```
13.Click the ​ + Insert Rule ​ link to edit the rule.
```
```
14.Under ​ IF (all match) ​, click ​ + Add condition ​.
```
```
15.Select ​ Requests otherwise not routed ​ from the menu.
```
```
16.Click the checkmark icon.
```
```
17.Under ​ THEN ​, click ​ + Add condition ​.
```
```
18.Select​ Forward to... ​, then select the name of your instances’ listener from the menu.
```
```
19.Click the checkmark icon.
```
#### Create the file stores with Elastic File System (EFS)

Create shared file stores for your Splunk Phantom cluster. Cluster nodes will store files that
must be shared by all instances to these shares. See ​Supported file systems and required
directories​ for more information.

**Note** ​: Only instance in the VPC you select during EFS creation can connect to that file system.
Make sure to use the same VPC for your EFS storage as you used for your Splunk Phantom
instances.

1. Under ​ **Configure file system access** ​, select the desired ​ **VPC** ​ from the menu.

```
Page 48 of 84
© 2019 Splunk Inc. All rights reserved.
```

2. Under ​ **Create mount targets** ​, select the check boxes for the availability zones you
    need.
3. Click ​ **Next Step** ​.
4. Set the security groups as required by your organization’s policies.
5. Under ​ **Configure optional settings** ​, set options as required by your organization’s
    requirements or policies.
6. Click ​ **Next Step** ​.
7. Review the options selected, then click ​ **Create File System** ​.

#### Create the external PostgreSQL database with the Relational Database System (RDS)

Splunk Phantom uses a PostgreSQL 9.4 database. In many installations, the database runs on
the same server as Splunk Phantom. For an AWS cluster, it makes sense to set up an external
PostgreSQL database using RDS. This database will serve as the primary database for the
Splunk Phantom cluster.

1. From your EC2 dashboard, click ​ **Services** ​ in the menu bar, and under ​ **Database** ​ choose
    **RDS** ​.
2. Click ​ **Create database** ​.
3. Select ​ **Standard Create** ​.
4. Under ​ **Engine options** ​, select ​ **PostgreSQL** ​.
5. For ​ **Version** ​, select ​ **9.4.20-R1** ​ from the menu.
6. For ​ **Templates** ​, select either ​ **Production** ​ for production environments or ​ **Dev/Test** ​ for
    development/testing or Proof of Value environments.
7. Under ​ **Settings** ​, type a name for your ​ **DB instance identifier** ​. Make sure that the name
    is unique across all DB instances owned by your AWS account.
8. Under Credential Settings:
    a. **Master username** ​: postgres

```
Page 49 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
b. Make sure the ​ Auto generate a password ​ check box is not selected.
c. Type and confirm the ​ Master password ​ in the fields provided. Record this
password. You will need it later.
```
9. Under ​ **DB instance size** ​, select the radio button that matches your organization’s needs.

```
Warning ​: Instances below db.t2.large may deplete their available connections before
installation of your Splunk Phantom cluster is complete.
```
10. Under ​ **Storage** ​, select a ​ **Storage type** ​ based on your organization’s needs.
    a. For ​ **Allocated storage** ​, set a number of GiB that matches your organization’s
       needs.

```
Warning ​: Splunk Phantom Databases below 500 gigabytes of storage are not
supported for production use.
```
```
b. Select the ​ Enable storage autoscaling ​ check box.
c. Set ​ Maximum storage threshold ​ to ​ 1000 ​ (GiB).
```
11.Under ​ **Availability & durability** ​, select the ​ **Do not create a standby instance** ​ radio
button.

12.Under ​ **Connectivity** ​, select the same ​ **VPC** ​ as you used for your Splunk Phantom
instances.

13.Under the ​ **Additional connectivity configuration** ​ section:
a. Select the correct ​ **Subnet group** ​. The available groups depend on your ​ **VPC**
selection.
b. Under ​ **Publicly accessible** ​, select the ​ **No** ​ radio button.
c. Under ​ **VPC security group** ​, select ​ **Choose existing** ​.
d. Select the appropriate security group from the menu.
e. Click the ​ **X** ​ icon to remove any unwanted security groups that were added by
default.
f. Make sure the ​ **Database port** ​ is set to ​ **5432** ​.

14.Under ​ **Additional configuration, Database options** ​:
a. Type ​ **phantom** ​ for ​ **Initial database name** ​.
b. Make sure the ​ **DB parameter group** ​ is set to ​ **default.postgres9.4** ​.

15.Under ​ **Additional configuration, Backup** ​, leave everything at the defaults.

```
Page 50 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
16.Click ​ Create Database ​.
```
#### Create the pgbouncer user for the RDS

Splunk Phantom interacts with the PostgreSQL database using the ​pgbouncer​ user account.
This account needs to be created for the database created in RDS.

1. Login to an AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
2. Elevate to ​root​.

```
sudo su -
```
3. Create the ​pgbouncer​ user.

```
psql --host <DNS name for RDS instance> --port 5432 --username
postgres --echo-all --dbname phantom --command "CREATE ROLE
pgbouncer WITH PASSWORD '<pgbouncer password>' login;"
```
4. Make the ​pgbouncer​ user a superuser.

```
psql --host <DNS name for RDS instance> --port 5432 --username
postgres --echo-all --dbname phantom --command "GRANT
rds_superuser TO pgbouncer;"
```
#### Add file shares to each Splunk Phantom instance

Set up and mount the needed directories for your Splunk Phantom cluster. Do this in two
stages. The first to create the required shared directories in EFS and copy over existing data,
the second to mount the directories on all Splunk Phantom instances and make the mounts
permanent.

**Stage one:**
Do this stage on only one of your AMI-based Splunk Phantom instances. You will create a
temporary directory, mount it to EFS, then use it to copy existing files to EFS.

1. Login to an AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
2. Elevate to ​root​.

```
sudo su -
```
```
Page 51 of 84
© 2019 Splunk Inc. All rights reserved.
```

3. Create a local mount on this instance. This mount will be used to replicate the required
    directory structure on EFS.

```
mkdir -p /mnt/external
```
4. Mount this directory on EFS.

```
mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=
2 <ip address or DNS name for EFS>:/ /mnt/external
```
5. Create the needed directories.

```
mkdir -p /mnt/external/apps
mkdir -p /mnt/external/app_states
mkdir -p /mnt/external/scm
mkdir -p /mnt/external/shared
mkdir -p /mnt/external/vault
```
6. Now copy the instance’s files to EFS with rsync.

```
Note ​: It can take several minutes or more to transfer all of the preinstalled apps.
```
```
rsync -avz /opt/phantom/apps /mnt/external/apps
rsync -avz /opt/phantom/local_data/app_states
/mnt/external/app_states
rsync -avz /opt/phantom/scm /mnt/external/scm
rsync -avz /opt/phantom/tmp/shared /mnt/external/shared
rsync -avz /opt/phantom/vault /mnt/external/vault
```
7. Unmount the temporary mounting.

```
umount /mnt/external
```
**Stage two** ​:
Do this stage on each of your AMI-based Splunk Phantom instances. Set the mounts for the
shared directories to EFS, then update the file system table to make the directories mount from
EFS when the instance starts.

1. Login to an AMI-based Splunk Phantom instance as the ​centos​ user using SSH.

```
Page 52 of 84
© 2019 Splunk Inc. All rights reserved.
```

2. Elevate to ​root​.

```
sudo su -
```
3. Mount all the shared directories to EFS.

```
mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=
2 <ip address or DNS name for EFS>:/apps /opt/phantom/apps
```
```
mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=
2 <ip address or DNS name for EFS>:/app_states
/opt/phantom/local_data/app_states
```
```
mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=
2 <ip address or DNS name for EFS>:/scm /opt/phantom/scm
```
```
mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=
2 <ip address or DNS name for EFS>:/shared /opt/phantom/shared
```
```
mount -t nfs4 -o
nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=
2 <ip address or DNS name for EFS>:/vault /opt/phantom/vault
```
4. Edit the file system table ​/etc/fstab​ to make the mounts permanent. Add these
    entries. You can get the EFS ID from your EFS dashboard.

```
vi /etc/fstab
```
```
<EFS ID>:/apps /opt/phantom/apps efs defaults,_netdev 0 0
<EFS ID>:/app_states /opt/phantom/local_data/app_states efs
defaults,_netdev 0 0
<EFS ID>:/scm /opt/phantom/scm efs defaults,_netdev 0 0
<EFS ID>:/shared /opt/phantom/tmp/shared efs defaults,_netdev 0
0
<EFS ID>:/vault /opt/phantom/vault efs defaults,_netdev 0 0
```
```
Page 53 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### Convert an AMI-based Splunk Phantom instance into the Splunk Enterprise instance

A Splunk Phantom cluster requires either a Splunk Enterprise instance or a distributed Splunk
Enterprise deployment as its search endpoint. Convert one of your AMI-based Splunk Phantom
instances into the required Splunk Enterprise endpoint.

If you already have a Splunk Enterprise deployment that you will use instead, follow the
instructions for using an external Splunk Enterprise instance found in the chapter ​Run Splunk
Phantom using external services​.

Convert Splunk Phantom instance into the Splunk Enterprise instance:

1. Login to the AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
2. Elevate to ​root​.

```
sudo su -
```
3. Run the ​make_server_node.pyc​ script with the ​splunk​ argument.

```
phenv python /opt/phantom/bin/make_server_node.pyc splunk
```
The Splunk Enterprise configuration is written to:
/opt/phantom/bin/splunk_config.json

Logs are written to:
/var/log/phantom/make_server_node/make_server_node_<date and
time>.log

#### Test each Splunk Phantom instance for readiness

Before proceeding, test each instance to make sure it is ready for conversion to a cluster node.
Log in to each AMI-based Splunk Phantom instance that will become a cluster node, perform
the tests, then fix any errors.

1. Make sure that each instance has the correct ​prepapre_db.py​ in
    /opt/phantom/bin​.

```
sudo ls -al /opt/phantom/bin/prepare_db.py
```
```
Page 54 of 84
© 2019 Splunk Inc. All rights reserved.
```

2. Make sure that each instance has the EFS file shares mounted.

```
sudo df -T
```
```
You must see entries for shared directories in the table with the ​<EFS
ID.dns_name>:/​ for the directories ​ apps ​, ​ app_states ​, ​ scm ​, ​ shared ​, and ​ vault ​.
```
3. Make sure that the instance is connecting to RDS as ​pgbouncer​.

```
sudo psql -h /tmp -p 5432 -d 'phantom'​ ​-c 'SELECT
current_user;'
```
```
You must see the output:
```
```
current_user
--------------
pgbouncer
(1 row)
```
#### Convert the first AMI-Based Splunk Phantom instance into a cluster node

Convert the first instance to a Splunk Phantom cluster node.

**Caution:** ​ Converting a Splunk Phantom installation to a server or cluster node is a one-way
operation. It cannot be reverted.

You will need this information readily available:
● IP or hostname for the RDS Postgres 9.4 DB server
● Password for the ​postgres​ user
● Password for the ​pgbouncer​ user
● IP or hostname of the ELB load balancer
● IP or hostname of the Splunk Enterprise instance
● REST API port for Splunk Enterprise: 5122
● User name for Splunk Phantom Search: ​phantomsearch
● Password for the ​phantomsearch​ account
● User name for Splunk Phantom Search: ​phantomdelete
● Password for the ​phantomdelete​ account
● HTTP Event Collector Token
● HTTP Event Collector port: 5121

The information for the Splunk Enterprise instance can be found in the file
/opt/phantom/bin/splunk_config.json​ on your Splunk Enterprise instance.

```
Page 55 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### Run make_cluster_node.pyc

##### Run make_cluster_node.pyc

1. Login to the AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
2. Elevate to ​root​.

```
sudo su -
```
3. Run the ​make_cluster_node.pyc​ script with the ​--record​ argument.

```
phenv python /opt/phantom/bin/make_cluster_node.pyc --record
```
The response file is written to: ​/opt/phantom/bin/response.json

The log is written to:
/var/log/phantom/make_cluster_node/make_cluster_node_<date and
time>.log

The response file can be used with the ​make_cluster_node.pyc​ script on other nodes to
automatically provide the information the script needs.

#### Convert the remaining AMI-based Splunk Phantom instances into cluster nodes

Convert each of the remaining AMI-based Splunk Phantom instances into cluster nodes by
running the ​make_cluster_node.pyc​ script.

##### Run make_cluster_node.pyc

1. Login to the AMI-based Splunk Phantom instance as the ​centos​ user using SSH.
2. Elevate to ​root​.

```
sudo su -
```
3. Run the ​make_cluster_node.pyc​ script with the ​--responses​ argument.

```
phenv python /opt/phantom/bin/make_cluster_node.pyc --responses
/opt/phantom/bin/responses.json
```
```
Note: ​ You don’t have to use ​responses.json​. If you do not supply a JSON file, the
Page 56 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
script prompts you for the information it needs.
```
#### Log in to the Splunk Phantom web interface

Connect to the web interface of your newly installed Splunk Phantom cluster.

1. Get the public IP address for your load balancer from the EC2 Management Console.
2. Using a browser, go to the public IP address.
    a. User name: ​admin
    b. Password: ​password
3. Change the admin user’s password:
    a. From the ​ **User Name** ​ menu, select ​ **Account Settings.**
    b. From the second level of the menu bar, select ​ **Change Password** ​.
    c. Type the current password.
    d. Type a new password.
    e. Type a new password a second time to confirm.
    f. Click ​ **Change Password** ​.

```
Page 57 of 84
© 2019 Splunk Inc. All rights reserved.
```

## Run Splunk Phantom using external services

Splunk Phantom supports the ability to run with the PostgreSQL database, file shares, and
Splunk components served by external servers.

### Set up the external PostgreSQL server

Splunk Phantom uses a PostgreSQL 9.4 database. In many installations, the database runs on
the same server as Splunk Phantom. It is possible to put the database on its own server. For
more information about configuring and operating a PostgreSQL database, consult the
PostgreSQL website and their documentation.

**Note:** ​ If you run the PostgreSQL database on its own server, install and configure PostgreSQL
before installing Splunk Phantom.

1. Install and configure one of the supported operating systems according to your
    organization’s requirements. See “Supported operating systems.”
2. Update the kernel semaphore parameters and refresh the system configuration.

```
echo "kernel.sem=250 32000 32 5000" >> /etc/sysctl.conf
sysctl --system
```
3. Configure your firewall to allow access.
    For a complete list of ports, see “Splunk Phantom required ports.”
4. Add any additional ​yum​ repositories that you need.
    Use the tool on at ​https://www.postgresql.org/download/linux/redhat/​ to identify the
    correct repository for your architecture and operating system combination.

```
yum install <URL>
```
5. Install the PostgreSQL server.

```
yum install postgresql94-server
```
6. Initialize the PostgreSQL database.

```
/usr/pgsql-9.4/bin/postgresql94-setup initdb
```
```
Page 58 of 84
© 2019 Splunk Inc. All rights reserved.
```

7. Set PostgreSQL to start when the system starts.

```
systemctl enable postgresql-9.4
```
8. Change to the ​postgres​ user.

```
sudo - postgres
```
9. Change to the PostgreSQL data directory.

```
cd /var/lib/pgsql/9.4/data
```
10.Generate the SSL certificate PostgreSQL uses.

```
Note: ​ You can use an SSL certificate purchased from a Certificate Authority instead of
generating a self-signed certificate.
```
```
openssl req -new -x509 -days 3650 -nodes -text -out server.crt
-keyout server.key -subj "/CN=postgres.cluster1"
```
11.Set the permissions on the ​server.key​ file.

```
chmod og-rwx server.key
```
12.Run ​psql​ as the ​postgres​ user.

```
sudo -u postgres psql
```
13.Set the ​postgres​ user password, if it has not already been set.

```
ALTER USER postgres PASSWORD '<postgrespassword>';
```
14.Create the ​pgbouncer​ user.

```
CREATE USER pgbouncer PASSWORD '<pgbouncerpassword>';
```
15.Set PostgreSQL to use SSL. Provide the keys and cipher level.

```
ALTER SYSTEM SET ssl = on
```
```
ALTER SYSTEM SET ssl_cert_file =
```
```
Page 59 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
'​/var/lib/pgsql/9.4/data/server.crt​'
```
```
ALTER SYSTEM SET ssl_key_file =
'​/var/lib/pgsql/9.4/data/server.key​'
```
```
ALTER SYSTEM SET ssl_ciphers = 'HIGH:+3DES:!aNULL'
```
16.Exit ​psql​ by typing ​ **CTRL+D** ​.

17.Change back to the ​root​ user.

```
exit
```
18.Edit the ​pg_hba.conf​ file to enable access to the database. Splunk Phantom must be
able to connect as both the ​postgres​ and ​pgbouncer​ users. In each entry, supply
the IP range that will be used by your Splunk Phantom install or cluster.

```
# TYPE DATABASE USER ADDRESS METHOD
local all all peer
hostssl all postgres <IP Range>/<XX> md5
hostssl phantom pgbouncer <IP Range>/<XX> md5
```
19.Edit ​postgresql.conf​. Set values for ​max_connections​, ​work_mem​,
shared_buffers, ​and​ listen_address.

```
max_connections=2500
work_mem=128kB
shared_buffers=2GB
listen_addresses = '*' # what IP address(es) to listen on;
```
```
For ​listen_address​ set a value ​that matches your security requirements. Valid
settings are:
● *​ for all addresses, ​0.0.0.0​ for all IPv4 addresses
● ::​ for all IPv6 addresses
● specific addresses you supply.
```
20.Restart the PostgreSQL service.

```
systemctl restart postgresql-9.4
```
```
Page 60 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Set up external file shares using GlusterFS

Splunk Phantom uses several volumes for storage. Splunk Phantom implements GlusterFS for
scalability and security of its file shares. You can put these volumes on their own server, or any
server that has adequate storage and bandwidth.

**Note:** ​ You can use other file systems to provide shared storage for Splunk Phantom. Any file
system that meets your organization’s security and performance requirements is sufficient. You
need to configure the required mounts and permissions. See ​Supported file systems​.

You can run GlusterFS as an expandable cluster of servers which provide a single mount point
for access. While you can run GlusterFS on a single server, three or more servers provides
more options for redundancy and high availability.

**Note:** ​ These instructions cover only configuring a single server and the required shares for
Splunk Phantom. To achieve high availability, data redundancy, and other features of GlusterFS
see ​https://docs.gluster.org​.

#### Prepare the GlusterFS server

1. Install and configure one of the supported operating systems according to your
    organization’s requirements.
2. Install the prerequisites.

```
yum install -y wget curl ntp
```
3. Synchronize the system clock.

```
ntpdate -v -u 0.centos.pool.ntp.org
```
4. Configure your firewall to allow access for Splunk Phantom nodes and other members of
    your GlusterFS cluster. For a complete list of ports, see “Splunk Phantom required
    ports.”
5. Format and mount the storage partition.

```
This partition must be separate from the operating system partition. The partition must
be formatted with a file system that supports extended attributes.
```
```
mkfs.xfs /dev/sdb1
Page 61 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
mkdir -p /data/gluster
echo '/dev/sdb1 /data/gluster xfs defaults 0 0' >> /etc/fstab
mount -a && mount
```
6. Install the phantom-base repository.

```
CentOS or RHEL version 6:
rpm -Uvh https://repo.phantom.us/phantom/4.6/base/6/x86_64/
phantom_repo-4.6.19142.x86_64.rpm
```
```
CentOS or RHEL version 7:
rpm -Uvh https://repo.phantom.us/phantom/4.6/base/7/x86_64/
phantom_repo-4.6.19142.x86_64.rpm
```
7. Update yum.

```
yum update
```
8. Install GlusterFS server.

```
yum install -y glusterfs-server-4.1.6-1.el7
```
9. Start the GlusterFS daemon and set it to start at boot.

```
systemctl start glusterd
systemctl enable glusterd
```
#### Prepare TLS certificates

1. Create the TLS certificates for GlusterFS.

```
openssl genrsa -out /etc/ssl/glusterfs.key 2048
```
2. Generate the ​.pem ​key for GlusterFS. You can use a certificate from a CA instead of
    generating a self-signed certificate.

```
openssl req -new -x509 -days 3650 -key /etc/ssl/glusterfs.key
-subj '/CN=gluster' -out /etc/ssl/glusterfs.pem
```
3. Copy the ​glusterfs.pem​ file to a ​.ca​ file.

```
Page 62 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
cp /etc/ssl/glusterfs.pem /etc/ssl/glusterfs.ca
```
4. Set ownership, read, write, and execute permissions on the ​glusterfs.key​ file.

```
chown <user>:<group> /etc/ssl/glusterfs.key
chmod o-rwx /etc/ssl/glusterfs.key
```
5. Create the directory and control file to make GlusterFS use TLS.

```
mkdir -p /var/lib/glusterd/
touch /var/lib/glusterd/secure-access
```
6. Copy the files for the TLS configuration. Store the copies in a safe place.

```
Note: ​ You will need these files to connect client machines to the file share.
```
```
tar -C /etc/ssl -cvzf glusterkeys.tgz glusterfs.ca
glusterfs.key glusterfs.pem
```
#### Configure the shared volumes

1. Create the shared directories used by Splunk Phantom.

```
cd /data/gluster/
mkdir -p apps app_states scm tmp/shared vault
```
2. Create the volumes in GlusterFS from the directories. Repeat for each volume: ​apps​,
    app_states​, ​scm​, ​tmp/shared​, and ​vault​.

```
gluster volume create <volume name> transport tcp <GlusterFS
hostname>:/data/gluster/<volume name> force
```
3. Activate SSL/TLS for each volume. Repeat for each volume; ​apps​, ​app_states​, ​scm​,
    tmp/shared​, and ​vault​.

```
gluster volume set <volume name> client.ssl on
gluster volume set <volume name> server.ssl on
gluster volume set <volume name> auth.ssl-allow '*'
```
4. Start each volume. Repeat for each volume: ​apps​, ​app_states​, ​scm​, ​tmp/shared​,
    and ​vault​.

```
Page 63 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
gluster volume start <volume name>
```
#### Configure Splunk Phantom cluster nodes to connect to the GlusterFS file shares

Each Splunk Phantom node in your cluster must have the same TLS keys stored in
/etc/ssl/​. Make sure to use the keys generated during GlusterFS installation.

If you are using the Splunk Phantom GUI to add new cluster nodes, you will need to supply the
TLS keys in ​ **Administration** ​ > ​ **Product Settings** ​ > ​ **Clustering** ​.

1. Create the directory and control file to make GlusterFS use TLS.

```
mkdir -p /var/lib/glusterd/
touch /var/lib/glusterd/secure-access
```
2. Copy your ​glusterkeys.tgz​ file to ​/etc/ssl/​ on the Splunk Phantom instance.
3. Extract the tar file.

```
tar xvzf glusterkeys.tgz
```
4. Delete the ​glusterkeys.tgz​ file from ​/etc/ssl/​.

#### Sync Splunk Phantom cluster nodes to the shared volumes

Splunk Phantom nodes must sync their local files to your newly shared volumes. The local
directories for ​apps​, ​app_states​, ​scm​, ​tmp/shared​, and ​vault​ contain files that need to
be preserved for use by your Splunk Phantom instance or cluster.

**Caution** ​: In a clustered environment, data only needs to be synced from the first node. Syncing
data from additional nodes will overwrite data from the first node.

1. Stop Splunk Phantom services on each node of the cluster.

```
stop_phantom.sh
```
2. Mount the local volumes to a temporary directory.

```
mkdir -p /tmp/phantom/<volume>
mount -t glusterfs <hostname of external file share>:<glusterfs
Page 64 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
volume name> /tmp/phantom/<volume>
```
3. Sync local data to the temporary location.

```
rsync -ah --progress <path/to/local/volume>
/tmp/phantom/<volume>/
```
```
Repeat for each volume: ​apps​, ​app_states​, ​scm​, and ​tmp/shared​.
```
4. Sync the ​vault​.

```
rsync -ah --exclude tmp --exclude chunks --progress
<path/to/local/vault> /tmp/phantom/vault
```
```
Sync the ​vault​ separately because it often contains very large amounts of data.
```
5. Unmount the temporary volumes. Repeat for each volume: ​apps​, ​app_states​, ​scm​,
    tmp/shared​, and ​vault​.

```
umount /tmp/phantom/<volume>
```
6. Edit the cluster member’s file system table, ​/etc/fstab​, to mount the GlusterFS
    volumes. Your ​fstab​ entries must not have line breaks.

```
<glusterfs_hostname>:/apps /<phantom_install_dir>/apps glusterfs
defaults,_netdev 0 0
```
```
<glusterfs_hostname>:/app_states
/<phantom_install_dir>/local_data/app_states glusterfs
defaults,_netdev 0 0
```
```
<glusterfs_hostname>:/scm /<phantom_install_dir>/scm glusterfs
defaults,_netdev 0 0
```
```
<glusterfs_hostname>:/tmp /<phantom_install_dir>/tmp/shared glusterfs
defaults,_netdev 0 0
```
```
<glusterfs_hostname>:/vault /<phantom_install_dir>/vault glusterfs
defaults,_netdev 0 0
```
7. Mount all the volumes to make them available.

```
mount /<phantom_install_dir>/apps
```
```
Page 65 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
mount /<phantom_install_dir>/local_data/app_states
mount /<phantom_install_dir>/scm
mount /<phantom_install_dir>/tmp/shared
mount /<phantom_install_dir>/vault
```
8. Start Splunk Phantom services on all cluster nodes.

```
start_phantom.sh
```
### Set up a load balancer with an HAProxy® server

A Splunk Phantom uses HAProxy as a load balancer to distribute requests between instances.

You can use a different load balancer. Configure your load balancer to provide round-robin
balancing, support SSL/TLS, and redirection from HTTP to HTTPS services.

If you use a different load balancer when creating a Splunk Phantom cluster, helpful
configuration information is written to the file ​ha_proxy_config_instructions.txt​ in
<phantom_install_dir>/bin/​.

1. Install and configure one of the supported operating systems according to your
    organization’s requirements.
2. Update SELinux and any firewalls to allow access to the ports for HAProxy, and your
    Splunk Phantom cluster nodes.
3. Install HAProxy.

```
yum install haproxy
```
4. Add SSL/TLS certificates to ​/etc/haproxy/certificates​.

```
Note: ​ Do not use a self-signed certificate in a production environment.
```
5. Edit ​/etc/haproxy/haproxy.cfg​. If the file does not exist, create it. Use the
    example file ​HAProxy Configuration​ as a guide. If you are creating an unprivileged
    cluster, make sure to include a directive for your custom HTTPS port.
6. Set HAProxy to start when the system starts.

```
systemctl enable haproxy.service
```
```
Page 66 of 84
© 2019 Splunk Inc. All rights reserved.
```

7. Start HAProxy.

```
systemctl start haproxy.service
```
For more detailed information on configuring an HAProxy server, consult the HAProxy
documentation on the HAProxy.org website.

### Set up Splunk Enterprise

If Splunk Phantom is installed as a stand-alone product, it includes a version of Splunk
Enterprise as the internal search engine. You can also configure Splunk Phantom to use an
external Splunk instance for searching.

A Splunk Phantom cluster also requires an external Splunk Enterprise instance.

Splunk Phantom 4.6 and later require Splunk Enterprise 7.2.3 or later and the Phantom Remote
Search App 1.0.7 or later. The Phantom Remote Search App defines the user roles and indices
needed by Splunk Phantom to use an external Splunk Enterprise deployment for searches.

#### Install Splunk Enterprise and add-ons

1. Install and configure Splunk Enterprise from the documentation. See the ​Splunk
    Enterprise Installation Manual​.
2. Configure your firewall to allow access. For a complete list of ports, see “Splunk
    Phantom required ports.”
3. Install the ​Phantom Remote Search App​. See the ​Splunk Enterprise Admin Manual,
    Where to get more apps and add-ons​.
4. Set up the HTTP Event Collector in Splunk. See the Splunk Enterprise manual, ​Getting
    Data In​.

#### Create required user accounts for Splunk Phantom

Splunk Phantom requires two user accounts with roles added by the Phantom Remote Search
App. The roles are ​phantomsearch​ and ​phantomdelete​. You can use any user names you
like for these accounts. These instructions use ​phantomsearch​ and ​phantomdelete​.

Create these accounts on a search head. These users will be replicated to the rest of the cluster

##### automatically. See ​Add users to the search head cluster​, in ​ Distributed Search ​.

```
Page 67 of 84
© 2019 Splunk Inc. All rights reserved.
```

1. Log in to Splunk Web.
2. Select ​ **Settings** ​ > ​ **Access Controls**
3. Click ​ **Users** ​.
4. Click ​ **New User** ​.
5. Type ​phantomsearch​ for ​ **Name** ​.
6. Set and confirm a password for this user which complies with your organization’s
    security policies.
7. Under ​ **Assigned role(s)** ​, in the ​ **Selected item(s)** ​ box, select ​user​ to remove that role.
8. Under ​ **Assigned role(s)** ​, in the ​ **Available item(s)** ​ box, select ​phantomsearch​ to add
    that role.
9. Deselect the ​ **Require password change on first login** ​ check box.

```
10.Click ​ Save ​.
```
```
11.Click ​ New User ​.
```
```
12.Type ​phantomdelete​ for ​ Name ​.
```
```
13.Set and confirm a password for this user which complies with your organization’s
security policies.
```
```
14.Under ​ Assigned role(s) ​, in the ​ Selected item(s) ​ box, select ​user​ to remove that role.
```
```
15.Under ​ Assigned role(s) ​, in the ​ Available item(s) ​ box, select ​phantomdelete​ to add
that role.
```
```
16.Deselect the ​ Require password change on first login ​ check box.
```
```
17.Click ​ Save ​.
```
#### Configure Splunk Phantom instances to use external Splunk Enterprise

Once the Phantom Remote Search app has been installed and the required user accounts have
been created, configure Splunk Phantom to use the external Splunk Enterprise instance.

```
Page 68 of 84
© 2019 Splunk Inc. All rights reserved.
```

**Note:** ​ You need a Splunk Enterprise license to use external Splunk Enterprise with Splunk
Phantom. If you do not already have one, please work with your Delivery Team to purchase
one.

**Prerequisites**
You need the host name and the REST API port number of your Splunk Enterprise instance, the
HTTP Event Collector token, and the user names and passwords for the user accounts with the
​phantomsearch​​ and ​​phantomdelete​​ roles.

Log in to Splunk Phantom as an administrative user.

1. From the ​​ **Main Menu** ​​, select ​​ **Administration** ​​.
2. Select ​ **Administration Settings** ​​.
3. Select ​ **Search Settings** ​​.
4. From ​​ **Search Endpoint** ​​, select the radio button for ​ **External Splunk Enterprise**
    **Instance** ​​.
5. In the ​ **Enable Splunk Search Endpoint** ​ section, type the host name of your Splunk
    Enterprise instance in the ​​ **Host** ​​ field.
6. In ​ **Access** ​ > ​ **User with Search Privileges** ​, type the user name and password for the
    user account with the ​​phantomsearch​​ role in the ​Username​ and ​Password​ fields.
7. In ​ **Access** ​ > ​ **User with Delete Privileges** ​, type the user name and password for the
    user account with the ​​phantomdelete​​ role in the ​Username​ and ​Password​ fields.
8. Type the port number that Splunk Enterprise uses to listen for REST API calls in the
    **REST Port** ​​ field.
9. Select the ​​ **Use SSL for REST** ​​ check box to enable SSL for REST API calls.

```
10.Select the ​ Verify Certificate for REST ​ check box to enable SSL certificate verification.
```
```
11.Type the port number for the Splunk Enterprise HTTP Event Collector in the ​​ HTTP
Event Collector Port ​​ field.
```
```
Page 69 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
12.Select the ​​ Use SSL for HTTP Event Collector ​​ check box to enable SSL for the HTTP
Event Collector.
```
```
13.Select the ​ Verify Certificate for HTTP Event Collector ​ check box to enable SSL
certificate verification.
```
```
14.Paste the HTTP Event Collector token in the ​​ HTTP Event Collector Token ​​ field.
```
15. Test the connection to your Splunk Enterprise instance by clicking ​ **Test Connection** ​.

```
16.Click ​​ Save Changes ​​.
```
#### Configure Splunk Phantom to use a distributed Splunk Enterprise deployment

Once the Phantom Remote Search app has been installed and the required user accounts have
been created, configure Splunk Phantom to use the distributed Splunk Enterprise deployment.

**Prerequisites**
● Search head information:
○ Host names for each search head.
○ REST API port numbers for each search head.
○ The user names and passwords for the user accounts with the ​phantomsearch
and ​phantomdelete​ roles.
● Indexer information:
○ Host names for each indexer.
○ The HTTP Event Collector port number.
○ The HTTP Event Collector token.

Add up to 10 search heads and 10 indexers.

1. Log in to Splunk Phantom as an administrative user.
2. From the ​​ **Main Menu** ​​, select ​​ **Administration** ​​.
3. Select ​ **Administration Settings** ​​.
4. Select ​ **Search Settings** ​​.

```
Page 70 of 84
© 2019 Splunk Inc. All rights reserved.
```

##### 5. From ​​ Search Endpoint ​​, select the radio button for ​ Distributed Splunk Enterprise

##### Deployment ​​.

6. Click ​ **Search Heads** ​.
7. Click ​ **Host** ​ to expand the ​ **Search Heads** ​ configuration.
8. In the ​ **Search Heads** ​ section, configure the first search head.

```
a. Type the search head host name in the ​ Host ​field.
b. In the ​ User with Search Privileges ​ section, type the user name and password
for the user account with the ​​phantomsearch​​ role in the ​​ Username ​​ and
​ Password ​​ fields.
c. In the ​ User with Delete Privileges ​ section, type the user name and password
for the user account with the ​​phantomdelete​​ role in the ​​ Username ​​ and
​ Password ​​ fields.
d. Type the port number that Splunk Enterprise uses to listen for REST API calls in
the ​ REST Port ​​ field.
e. Select the ​ Use SSL for REST ​ check box to enable SSL.
f. Select the ​ Verify Certificate for REST ​ check box to enable SSL certificate
verification.
```
9. Click ​ **Add Search Head** ​ to configure additional search heads. Each time you add a new
    search head, it will be prepopulated with the user names and passwords for the user
    accounts with the ​phantomsearch​ and ​phantomdelete​ roles of the previous search
    head.

10.Once all the search heads have been added, click ​ **Indexers** ​.

11.Click ​ **Host** ​ to expand the ​ **Indexers** ​ configuration.

12.In the ​ **Indexers** ​ section, configure the first indexer.

```
a. Type the indexer host name in the ​ Host ​field.
b. Type the HTTP Event Collector port in the ​ HTTP Event Collector Port ​ field.
c. Paste the HTTP Event Collector token in the ​ HTTP Event Collector Token ​ field.
d. Select the ​​ Use SSL for HTTP Event Collector ​​ check box to enable SSL.
e. Select the ​ Verify Certificate for HTTP Event Collector ​ check box to enable
SSL certificate verification.
```
```
Page 71 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
13.Click ​ Add Indexer ​ to configure additional indexers.
```
```
14.Once all search heads and indexers have been added, test the connections to your
distributed Splunk Enterprise deployment by clicking ​ Test Connection ​.
```
```
15.Click ​​ Save Changes ​​.
```
## Upgrade Splunk Phantom

The process of upgrading a Splunk Phantom deployment is straightforward. There are some
differences for clustered environments or unprivileged deployments, but the process is largely
the same.

The amount of required downtime for an upgrade will depend on the amount of data in your
database, and the release version to which you are upgrading.

### Prerequisites

You will need the following items before beginning your upgrade.
● A user account on the operating system for your Splunk Phantom instance or cluster
nodes with sudo privileges, or root access on those systems.
● Your Splunk Phantom Community portal login. The script will prompt you for these
credentials during the upgrade.

#### Suggested preparations

Make a full backup of your Splunk Phantom deployment before upgrading. Alternatively, for
single instance deployments running as a virtual machine, create a snapshot of the virtual
machine.

After your upgrade is complete, clear your browser cache before logging into Splunk Phantom
for the first time.

### Upgrade a standalone Splunk Phantom instance

Follow these steps to upgrade your Splunk Phantom instance from version 4.5.15922 to version
4.6.19142.

```
Page 72 of 84
© 2019 Splunk Inc. All rights reserved.
```

1. Log in to the Splunk Phantom instance’s operating system as either the root user or a
    user with sduo privileges.
2. Stop all Splunk Phantom services.

```
sudo /opt/phantom/bin/stop_phantom.sh
```
3. Clear the YUM caches.

```
yum clean all
```
4. Update the operating system and installed packages.

```
yum update
```
5. If a kernel update was included in your operating system updates, restart the operating
    system.

```
sudo shutdown -r now
```
6. After the system restarts, log in to the operating system as either the root user or a user
    with sudo privileges.
7. Install the Splunk Phantom repository and signing keys.

```
CentOS 6 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/6/x86_64/phantom_repo-
4.6.19142-1.x86_64.rpm
```
```
RHEL 6 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/6Server/x86_64/phantom
_repo-4.6.19142-1.x86_64.rpm
```
```
CentOS 7 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/7/x86_64/phantom_repo-
4.6.19142-1.x86_64.rpm
```
```
RHEL 7 ​:
```
```
Page 73 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/7Server/x86_64/phantom
_repo-4.6.19142-1.x86_64.rpm
```
8. Run the upgrade script.

```
sudo /opt/phantom/bin/phantom_setup.sh upgrade
```
### Upgrade a Splunk Phantom cluster

Upgrading a Splunk Phantom cluster requires upgrading individual nodes, one at a time. Follow
these stages, in order, to upgrade your Splunk Phantom cluster from version 4.5.15922 to
version 4.6.19142.

1. On each node, upgrade the operating system and installed packages.
2. On each node, install the Splunk Phantom repositories and signing keys.
3. On each node, one at a time, upgrade the cluster node..

#### Upgrade the operating system and installed packages

1. Log in to the Splunk Phantom instance’s operating system as either the root user or a
    user with sduo privileges.
2. Stop all Splunk Phantom services.

```
sudo /opt/phantom/bin/stop_phantom.sh
```
3. Clear the YUM caches.

```
yum clean all
```
4. Update the operating system and installed packages.

```
yum update
```
5. If a kernel update was included in your operating system updates, restart the operating
    system.

```
sudo shutdown -r now
```
```
Page 74 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### Install the Splunk Phantom repositories and signing keys

1. Log in to the operating system as either the root user or a user with sudo privileges.
2. Install the Splunk Phantom repository and signing keys.

```
CentOS 6 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/6/x86_64/phantom_repo-
4.6.19142-1.x86_64.rpm
```
```
RHEL 6 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/6Server/x86_64/phantom
_repo-4.6.19142-1.x86_64.rpm
```
```
CentOS 7 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/7/x86_64/phantom_repo-
4.6.19142-1.x86_64.rpm
```
```
RHEL 7 ​:
rpm -Uvh
https://repo.phantom.us/phantom/4.6/base/7Server/x86_64/phantom
_repo-4.6.19142-1.x86_64.rpm
```
#### Upgrade individual cluster nodes

Do not attempt to upgrade all the nodes at the same time. Upgrade a single node and bring it
back online before proceeding to the next cluster node.

When you have upgraded N+1 nodes, where N is equal to half of the number of nodes in your
cluster, database migrations will be applied. This can take a significant amount of time,
depending on the amount of data in your database and the version of Splunk Phantom from
which you’re upgrading.

1. Log in to the operating system as either the root user or a user with sudo privileges.
2. Run the upgrade script.

```
sudo /opt/phantom/bin/phantom_setup.sh upgrade
```
```
Page 75 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Upgrade a standalone, unprivileged Splunk Phantom instance

Follow these steps to upgrade your unprivileged Splunk Phantom instance from version
4.5.15922 to version 4.6.19142.

1. Download the installation tar file from the Splunk Phantom Community portal. If you do
    not see the required file, contact Phantom support.
2. Log in to the Splunk Phantom instance’s operating system as either the root user or a
    user with sduo privileges.
3. Stop all Splunk Phantom services.

```
sudo /opt/phantom/bin/stop_phantom.sh
```
4. Clear the YUM caches.

```
yum clean all
```
5. Update the operating system and installed packages.

```
yum update
```
6. If a kernel update was included in your operating system updates, restart the operating
    system.

```
sudo shutdown -r now
```
7. After the system restarts, log in to the operating system as either the root user or a user
    with sudo privileges.
8. Copy the installation tar file to a subdirectory in the Phantom user’s directory,
    /home/<username>/<directory_name>​.
9. Extract the installation tar file.

```
tar -xvzf phantom-4.6.19142.tgz
```
```
Page 76 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
10.Run the upgrade script.
```
```
sudo /home/<username>/phantom_tar_install.sh upgrade
```
### Upgrade an unprivileged Splunk Phantom cluster

Upgrading an unprivileged Splunk Phantom cluster requires upgrading individual nodes, one at
a time, using a tar file obtained from the Splunk Phantom Community portal. Follow these
stages, in order, to upgrade your Splunk Phantom cluster from version 4.5.15922 to version
4.6.19142.

1. On each node, upgrade the operating system and installed packages.
2. On each node, copy and extract the installation tar file.
3. On each node, one at a time, upgrade the individual cluster nodes.

#### Upgrade the operating system and installed packages

1. Download the installation tar file from the Splunk Phantom Community portal. If you do
    not see the required file, contact Phantom support.
2. Log in to the Splunk Phantom instance’s operating system as either the root user or a
    user with sduo privileges.
3. Stop all Splunk Phantom services.

```
sudo /opt/phantom/bin/stop_phantom.sh
```
4. Clear the YUM caches.

```
yum clean all
```
5. Update the operating system and installed packages.

```
yum update
```
6. If a kernel update was included in your operating system updates, restart the operating
    system.

```
sudo shutdown -r now
```
```
Page 77 of 84
© 2019 Splunk Inc. All rights reserved.
```

#### Copy the installation tar file to the cluster node

1. Log in to the operating system as either the root user or a user with sudo privileges.
2. Copy the installation tar file to a subdirectory in the Phantom user’s directory,
    /home/<username>/<directory_name>​.
3. Extract the installation tar file.

```
tar -xvzf phantom-4.6.19142.tgz
```
#### Upgrade individual cluster nodes

Do not attempt to upgrade all the nodes at the same time. Upgrade a single node and bring it
back online before proceeding to the next cluster node.

When you have upgraded N+1 nodes, where N is equal to half of the number of nodes in you
cluster, database migrations will be applied. This can take a significant amount of time,
depending on the amount of data in your database and the version of Splunk Phantom from
which you’re upgrading.

1. Log in to the operating system as either the root user or a user with sudo privileges.
2. Run the upgrade script.

```
sudo /home/<username>/phantom_tar_install.sh upgrade
```
```
Page 78 of 84
© 2019 Splunk Inc. All rights reserved.
```

## Reference

This section has the default Splunk Phantom credentials, script options and example
configuration files.

### Default credentials

The default credentials on a new installation of Splunk Phantom are:
● Username: ​admin
● Password: ​password

The default credentials of a new AMI installation of Splunk Phantom are:
● Username: ​admin
● Password: ​<full AWS instance ID>

Change the password immediately after you install Splunk Phantom.

### Installation or configuration scripts

This section lists various installation scripts and their command line options.

#### phantom_tar_install.sh options

Use these arguments to control the ​phantom_tar_install.sh​ script. Only use this to
install Splunk Phantom as an unprivileged user.

**Option** ​: ​install
Install Splunk Phantom.

```
Argument Description
```
```
--https-port=HTTPS_PORT Set the custom HTTPS port for Splunk Phantom.
```
```
--no-prompt Run the script without a confirmation prompt.
```
```
--without-apps Do not install any of the apps that ship with Splunk
Phantom. Apps can be installed later using the GUI.
```
```
Page 79 of 84
© 2019 Splunk Inc. All rights reserved.
```

**Option** ​: ​upgrade
Upgrade an install of Splunk Phantom.

```
Argument Description
```
```
--no-prompt Run the script without a confirmation prompt.
```
```
--without-apps Do not install any of the apps that ship with Splunk
Phantom. Apps can be installed later using the GUI.
```
#### Phantom_setup.sh options

Use these arguments to control the ​phantom_setup.sh​ script.

**Option** ​: ​install
Install Splunk Phantom.

```
Argument Description
```
```
--no-prompt Run the script without confirmation prompt.
```
```
--without-apps Do not install any of the apps that ship with Splunk
Phantom. Apps can be installed later using the GUI.
```
```
--non-root Run the ​watchdog​ daemon with reduced privileges.
```
```
--yumopts=”...” Additional parameters to pass to ​yum​.
```
```
--mingit Installs a minimal Git package without the Perl Git
module.
```
```
--rhsc-psql Installs PostgreSQL from Red Hat Source
Collections.
```
```
--version Specify which version of Splunk Phantom to install.
```
```
Page 80 of 84
© 2019 Splunk Inc. All rights reserved.
```

**Option** ​: ​upgrade
Upgrade an install of Splunk Phantom.

```
Argument Description
```
```
--no-prompt Run the script without a confirmation prompt.
```
```
--without-apps Do not install any of the apps that ship with Splunk
Phantom. Apps can be installed later using the GUI.
```
```
--non-root Run the ​watchdog​ daemon with reduced privileges.
```
```
--yumopts=”...” Additional parameters to pass to ​yum​.
```
```
--version Specify which version of Splunk Phantom to install.
```
#### make_server_node.pyc options

Use these options to control the ​make_server_node.pyc​ command.

```
Argument Description
```
```
--version Displays the program’s version number.
```
```
--help Display a list and description of arguments.
```
```
--no-prompt Run the program. Do not display the warning prompt.
```
Convert an OVA install of Splunk Phantom into a server node for a cluster. A server node
provides one or more of the services a cluster requires, such as proxy, database, file share, or
search endpoint.

```
Argument Description
```
```
all This is the default option.
```
```
Install HAProxy, PostgreSQL, GlusterFS, and Splunk
on this node.
```
```
This option creates a best effort version of
mcn_responses.json​ to be used with
make_cluster_node.pyc​.
```
```
fs Create and configure a single node GlusterFS file
share on this node for Splunk Phantom.
```
```
Page 81 of 84
© 2019 Splunk Inc. All rights reserved.
```

```
The directory tree starts with
/opt/phantom/shared​.
```
```
db Create the Splunk Phantom PostgreSQL database on
this node to act as an external database.
```
```
proxy Install HAProxy to act as a load balancer for the
Splunk Phantom cluster.
```
```
splunk Install Splunk Enterprise to act as remote search
endpoint for the cluster.
```
#### make_cluster_node.pyc options

Convert an OVA install of Splunk Phantom into a cluster node for a cluster. A cluster node is a
single instance of Splunk Phantom supported by one or more server nodes.

```
Argument Description
```
```
--responses Send prepared responses from
mcn_responses.json​ or ​responses.json​ to the
script.
```
```
--record Create to a ​responses.json​ file to use when
running this script on another node. This version of
the file does contain passwords.
```
```
--record-no-secret Create to a ​responses.json​ file to use when
running this script on another node. This version of
the file does not contain passwords.
```
```
--record-location Set the location to record the ​responses.json​ file.
The default is
/opt/phantom/bin/response.json​.
```
```
--delete-responses-file Delete the responses file used from the machine
when the script completes.
```
```
--version Show the program version number and exit.
```
**Caution:** ​ Some versions of the file ​responses.json​ contain passwords.

```
Page 82 of 84
© 2019 Splunk Inc. All rights reserved.
```

### Configuration files

This section contains example configuration files. Use these as a guide when configuring items
for use in your Splunk Phantom deployment.

#### HAProxy Configuration

##--------------------------------------------------------------------
## HAPROXY 1.8.7 CONFIGURATION FILE
##--------------------------------------------------------------------
#---------------------------------------------------------------------
# global settings
#---------------------------------------------------------------------
global
tune.ssl.default-dh-param 2048
log 127.0.0.1:514 local0

#---------------------------------------------------------------------
# common defaults
#---------------------------------------------------------------------
defaults
mode http
timeout connect 0ms
timeout client 0ms
timeout server 0ms
log global

#---------------------------------------------------------------------
# SSL w/ redirect to HTTPS
#---------------------------------------------------------------------
frontend localhost
bind *:80
bind *:443 ssl crt /etc/haproxy/certificates no-sslv3 no-tlsv10 ciphers
<ciphers go here>
# for unprivileged installs, add another declaration
# bind *:<your https port> ssl crt /etc/haproxy/certificates no-sslv3
# no-tlsv10 ciphers <ciphers go here>
redirect scheme https if !{ ssl_fc }
mode http
default_backend nodes

#---------------------------------------------------------------------
# backend (output)
#---------------------------------------------------------------------
backend nodes

```
Page 83 of 84
© 2019 Splunk Inc. All rights reserved.
```

mode http
balance roundrobin
option http-keep-alive
option forwardfor
cookie SRVNAME insert
option httpchk GET /check HTTP/1.1\r\nHost:\ [http://www.example.com](http://www.example.com)
http-check expect status 200
default-server fastinter 1s downinter 5s
server <phantom node UUID> <IP Address>:443 cookie <phantom node UUID>
check ssl verify none
http-request set-header X-Forwarded-Port %[dst_port]
http-request add-header X-Forwarded-Proto https if { ssl_fc }

```
Page 84 of 84
© 2019 Splunk Inc. All rights reserved.
```

