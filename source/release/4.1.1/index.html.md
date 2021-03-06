---
title: oVirt 4.1.1 Release Notes
category: documentation
authors: sandrobonazzola
---

# oVirt 4.1.1 Release Notes

The oVirt Project is pleased to announce the availability of 4.1.1
First Release Candidate as
of March 03, 2017.

oVirt is an open source alternative to VMware™ vSphere™, and provides an
awesome KVM management interface for multi-node virtualization.
This release is available now for Red Hat Enterprise Linux 7.3,
CentOS Linux 7.3 (or similar).


This is pre-release software.
Please take a look at our [community page](/community/) to know how to
ask questions and interact with developers and users.
All issues or bugs should be reported via the
[Red Hat Bugzilla](https://bugzilla.redhat.com/enter_bug.cgi?classification=oVirt).
The oVirt Project makes no guarantees as to its suitability or usefulness.
This pre-release should not to be used in production, and it is not feature
complete.


To find out more about features which were added in previous oVirt releases,
check out the
[previous versions release notes](/develop/release-management/releases/).
For a general overview of oVirt, read
[the Quick Start Guide](Quick_Start_Guide)
and the [about oVirt](about oVirt) page.

[Installation guide](http://www.ovirt.org/documentation/install-guide/Installation_Guide/)
is available for updated and detailed installation instructions.

### Fedora / CentOS / RHEL


## RELEASE CANDIDATE

In order to install this Release Candidate you will need to enable pre-release repository.


In order to install it on a clean system, you need to install


`# yum install `[`http://resources.ovirt.org/pub/yum-repo/ovirt-release41-pre.rpm`](http://resources.ovirt.org/pub/yum-repo/ovirt-release41-pre.rpm)


and then follow our
[Installation guide](http://www.ovirt.org/documentation/install-guide/Installation_Guide/)



### oVirt Hosted Engine

If you're going to install oVirt as Hosted Engine on a clean system please
follow [Hosted_Engine_Howto#Fresh_Install](Hosted_Engine_Howto#Fresh_Install)
guide or the corresponding section in
[Self Hosted Engine Guide](/documentation/self-hosted/Self-Hosted_Engine_Guide/)

If you're upgrading an existing Hosted Engine setup, please follow
[Hosted_Engine_Howto#Upgrade_Hosted_Engine](Hosted_Engine_Howto#Upgrade_Hosted_Engine)
guide or the corresponding section within the
[Upgrade Guide](/documentation/upgrade-guide/upgrade-guide/)

### EPEL

TL;DR Don't enable all of EPEL on oVirt machines.

The ovirt-release package enables the epel repositories and includes several
specific packages that are required from there. It also enables and uses
the CentOS OpsTools SIG repos, for other packages.

EPEL currently includes collectd 5.7.1, and the collectd package there includes
the write_http plugin.

OpsTools currently includes collectd 5.7.0, and the write_http plugin is
packaged separately.

ovirt-release does not use collectd from epel, so if you only use it, you
should be ok.

If you want to use other packages from EPEL, you should make sure to not
include collectd. Either use `includepkgs` and add those you need, or use
`excludepkgs=collectd*`.


## What's New in 4.1.1?

### Enhancements

#### oVirt Cockpit Plugin

 - [BZ 1358716](https://bugzilla.redhat.com/1358716) <b>Disable Hosted Engine Setup page after register RHVH to engine</b><br>Feature: The hosted engine setup wizard now warns users if the host is already registered to RHV-M.<br><br>Reason: Previously, a host which was registered to RHV-M but not running hosted engine would present the option to set up hosted engine, which ran the risk of unregistering the host.<br><br>Result: Hosts which are registered to RHV-M now present a "Redeploy" button in the Hosted Engine wizard in cockpit, which must be selected in order to continue.
 - [BZ 1423542](https://bugzilla.redhat.com/1423542) <b>Include gdeploy preflight check</b><br>Feature: <br><br>  Gdeploy has a script to validate basic network setup and storage configuration before deploying gluster. This script should be included in the generated gdeploy config for HC deployment.<br><br>Reason:<br><br>It is recommended to run these checks before deploying Gluster in HC environment.<br><br>Result:<br><br>User will get a cleaner HC environment deployed.

#### oVirt Engine

 - [BZ 1413150](https://bugzilla.redhat.com/1413150) <b>[RFE] Add warning to change CL to the match the installed engine version</b><br>Feature: <br><br>Following warnings are provided for for all data centers/clusters which are not upgraded to latest version:<br><br>1. We have a new engine job, which iterates over existing data centers and check compatibility version of each data center. If data center is not upgraded to latest version, alert is raised and stored in audit log. This job executed after engine startup and then it's scheduled to execute once a week<br><br>2. In Data Centers main tab we show exclamation mark icon for each data center which is not upgraded to latest version (hovering over exclamation mark will show a reason for users).<br><br>3. In Clusters main  we show exclamation mark icon for each cluster which is not upgraded to latest version (hovering over exclamation mark will show a reason for users).<br><br>Reason: <br><br>Result:
 - [BZ 1408193](https://bugzilla.redhat.com/1408193) <b>[RFE] Update timestamp format in engine log to timestamp with timezone</b><br>Feature: <br>All timestamp records for engine and engine tools logs will from now contain a time zone to ease correlation between logs on engine and hosts. Previously engine.log contained timestamp without time zone:<br><br>2017-02-27 13:35:06,720 INFO  [org.ovirt.engine.core.dal.dbbroker.DbFacade] (ServerService Thread Pool -- 51) [] Initializing the DbFacade<br><br>From now on there will always be a timezone identifier at the end of timestamp part:<br><br>2017-02-27 13:35:06,720+01 INFO  [org.ovirt.engine.core.dal.dbbroker.DbFacade] (ServerService Thread Pool -- 51) [] Initializing the DbFacade<br><br>Reason: <br><br>Result:
 - [BZ 1424787](https://bugzilla.redhat.com/1424787) <b>'Available swap memory' warning should appear once a day, not every 30 minutes</b><br>
 - [BZ 1388430](https://bugzilla.redhat.com/1388430) <b>[RFE]  Provide a tool to execute vacuum full on engine database</b><br>- There is a documentation bug for this one - see Bug 1416049<br><br>Feature: <br>Adding a tool and a setup-plugin to run vacuum on the engine db. <br><br>Reason: <br>Adding a maintenance tool, to optimize tables stats and clean up garbage and compact the internals of tables. The result is less disk space usage, more efficient future maintenance work, updated table stats for better query planning.<br><br>Result: <br>A cli tools, secure and easy to work with that runs all sorts of vacuum actions on the engine db or specific tables.<br>A setup-plugin dialog that offers to perform vacuum on the engine while in this maintenance period, optionally automated by the answer file.
 - [BZ 1369175](https://bugzilla.redhat.com/1369175) <b>VM Console options: hide "Enable USB Auto-Share" entry when USB is disabled.</b><br>"Enable USB Auto-Share" option in "Console options" dialog is now enabled only if "USB Support" is checked for the VM.
 - [BZ 1406398](https://bugzilla.redhat.com/1406398) <b>[RFE] Add NFS V4.2 support for ovirt-engine</b><br>oVirt now supports NFS version 4.2 connections (where supported by storage)
 - [BZ 1412547](https://bugzilla.redhat.com/1412547) <b>Allow negotiation of highest available TLS version for engine <-> VDSM communication</b><br>Feature: <br><br>Currently when engine tries to connect to VDSM, it tries to negotiate highest available version of TLS, but due to issues in the past we have a limitation to try TLSv1.0 as highest version and not try any higher version.<br><br>This fix removes the limit, so we can negotiate also TLSv1.1 and TLSv1.2 when they will be available on VDSM side. Removing this limit will allow us to drop TLSv1.0 in future VDSM versions and provide only newer TLS versions<br><br><br>Reason: <br><br>Result:

#### VDSM

 - [BZ 1403839](https://bugzilla.redhat.com/1403839) <b>[RFE] Add ability to remove a single LUN from a data domain</b><br>Feature: <br>Adding ability to remove LUNs from a block data domain.<br><br>Reason: <br>The user may want to remove a device from a block data domain.<br><br>Result: <br>LUNs can be removed from block data domain as while as there's enough free space on the other domain devices to contain the data stored on those devices.

#### oVirt Hosted Engine HA

 - [BZ 1101554](https://bugzilla.redhat.com/1101554) <b>[RFE] HE-ha: use vdsm api instead of vdsClient</b><br>With this update, the code interfacing with VDSM now uses the VDSM API directly instead of using vdsClient and xmlrpc.

#### oVirt Host Deploy

 - [BZ 1408942](https://bugzilla.redhat.com/1408942) <b>Host deploy should require cockpit-ovirt.</b><br>oVirt Host Deploy now install and maintain updated cockpit-ovirt package on hypervisor hosts.

### No Doc Update

#### oVirt Cockpit Plugin

 - [BZ 1427450](https://bugzilla.redhat.com/1427450) <b>drop the UI plugin check for cockpit on hosts</b><br>

#### oVirt Engine

 - [BZ 1418537](https://bugzilla.redhat.com/1418537) <b>[Admin Portal] Exception while adding new host network QoS from cluster->logical networks->add network</b><br>

#### VDSM

 - [BZ 1215039](https://bugzilla.redhat.com/1215039) <b>[HC] - API schema for StorageDomainType is missing glusterfs type</b><br>

#### MOM

 - [BZ 1370081](https://bugzilla.redhat.com/1370081) <b>Deprecation warning about BaseException.message in a log</b><br>

### Unclassified

#### oVirt Cockpit Plugin

 - [BZ 1415655](https://bugzilla.redhat.com/1415655) <b>Arbiter flag should not be enabled by default in gdeploy wizard</b><br>
 - [BZ 1415657](https://bugzilla.redhat.com/1415657) <b>Do not allow the user to proceed further if he does not fill mandatory fields required for deployment.</b><br>
 - [BZ 1421249](https://bugzilla.redhat.com/1421249) <b>Cannot use cockpit for hosted-engine setup if gpg keys have not already been accepted</b><br>
 - [BZ 1363971](https://bugzilla.redhat.com/1363971) <b>Appliance root password prompt popup in browser when entering the appliance password in cockpit</b><br>
 - [BZ 1416175](https://bugzilla.redhat.com/1416175) <b>Remove the NTP server configuration from gdeploy</b><br>
 - [BZ 1416168](https://bugzilla.redhat.com/1416168) <b>NetworkManager need not be stopped and disabled</b><br>
 - [BZ 1426527](https://bugzilla.redhat.com/1426527) <b>Update the gluster volume options set via generated gdeploy config file</b><br>
 - [BZ 1415615](https://bugzilla.redhat.com/1415615) <b>conditional flag for pool id is required  in cockpit-ovirt gdeploy plugin.</b><br>
 - [BZ 1422935](https://bugzilla.redhat.com/1422935) <b>Packages text should be left blank and update host should be left unchecked by default</b><br>
 - [BZ 1424766](https://bugzilla.redhat.com/1424766) <b>Remove nrpe related configuration from gdeploy configuration</b><br>
 - [BZ 1421089](https://bugzilla.redhat.com/1421089) <b>cockpit-ovirt fails to build due to missing webpack</b><br>
 - [BZ 1411315](https://bugzilla.redhat.com/1411315) <b>Oops message displayed with an exclamation mark once user logs into cockpit UI</b><br>
 - [BZ 1416299](https://bugzilla.redhat.com/1416299) <b>remove vdsm configuration from gdeploy config</b><br>
 - [BZ 1416435](https://bugzilla.redhat.com/1416435) <b>Add  units to represent size of disks and add a text near no.of disks for RAID to indicate what disks they are</b><br>
 - [BZ 1416428](https://bugzilla.redhat.com/1416428) <b>Remove distribute and distribute-replicate  from volume types combo box or replace the combo box with a label called 'Replicate'</b><br>

#### oVirt Engine

 - [BZ 1419562](https://bugzilla.redhat.com/1419562) <b>[Sysprep] windows 2012 R2 and windows 2016 - failed to load sysprep file</b><br>A problem with Windows server operating system sysprep provisioning failing when the RHV configuration does not provide a valid Product Key was fixed. Using an empty key caused the provisioning to stop without completing other items, so the configuration section in Unattend.xml is now automatically removed.<br>Custom sysprep script is not affected as it is entirely provided by users. The expectation is that it does contain a valid key and a potential Product Key section is intentional.
 - [BZ 1421174](https://bugzilla.redhat.com/1421174) <b>Migration scheduler should work with per-VM cluster compatibility level</b><br>
 - [BZ 1414320](https://bugzilla.redhat.com/1414320) <b>oVirt reporting low disk space on template copy when there's still a plenty</b><br>
 - [BZ 1417582](https://bugzilla.redhat.com/1417582) <b>Provide a warning dialog for add brick operation for the volume which backs gluster data domain</b><br>
 - [BZ 1418247](https://bugzilla.redhat.com/1418247) <b>UI showing active geo-rep session, when the session was already removed from CLI</b><br>
 - [BZ 1379074](https://bugzilla.redhat.com/1379074) <b>[storage] Improve logging for ExportVM flow</b><br>
 - [BZ 1414818](https://bugzilla.redhat.com/1414818) <b>Update affinity group drop general positive flag and moves VM to VM affinity to disabled state</b><br>
 - [BZ 1401010](https://bugzilla.redhat.com/1401010) <b>Memory donut shows nonsense value</b><br>
 - [BZ 1418757](https://bugzilla.redhat.com/1418757) <b>Package list for upgrade checks has to contain only valid packages per version</b><br>
 - [BZ 1361223](https://bugzilla.redhat.com/1361223) <b>[AAA] Missing principal name option for keytab usage on kerberos</b><br>In BZ1322940 we have provided a way how to reuse GSSAPI configuration provided by application server. This fix adds an option how to specify principal name if multiple principal names are present within configured keytab.<br><br>This principal name can be specified using following variable:<br><br>AAA_JAAS_PRINCIPAL_NAME=principal_name<br><br>By default principal name is empty, which works fine for cases where only one principal is defined in specified keytab (most common cases).<br><br>To use that option, the user has to create a new configuration file and specify the correct values for GSSAPI variables (more information in BZ1322940), for example: /etc/ovirt-engine/engine.conf.d/99-jaas.conf.
 - [BZ 1415471](https://bugzilla.redhat.com/1415471) <b>Adding host to engine failed at first time but host was auto recovered after several mins</b><br>
 - [BZ 1414455](https://bugzilla.redhat.com/1414455) <b>removing disk in VM edit dialog causes UI error</b><br>
 - [BZ 1410606](https://bugzilla.redhat.com/1410606) <b>Imported VMs has max memory 0</b><br>
 - [BZ 1388456](https://bugzilla.redhat.com/1388456) <b>Disable TLSv1.0 in Apache SSL configuration</b><br>
 - [BZ 1406005](https://bugzilla.redhat.com/1406005) <b>[RFE][Metrics Store] Install Collectd and fluentd with relevant plugins on engine machine</b><br>
 - [BZ 1380356](https://bugzilla.redhat.com/1380356) <b>Engine confused by external network provider not responding to create-port command</b><br>
 - [BZ 1417554](https://bugzilla.redhat.com/1417554) <b>Mount options are not explicitly made visible, when the new storage domain is associated with gluster volume</b><br>
 - [BZ 1417571](https://bugzilla.redhat.com/1417571) <b>'Remote data sync setup' should throw helpful message, if there are no gluster geo-rep is setup for the volume</b><br>
 - [BZ 1417816](https://bugzilla.redhat.com/1417816) <b>Remote data sync setup should show up the scheduled time with 2 digit precision for hours and minutes</b><br>
 - [BZ 1418567](https://bugzilla.redhat.com/1418567) <b>Throw proper error/warning message, when removing the gluster geo-rep session associated with remote sync setup gluster data domain</b><br>
 - [BZ 1356949](https://bugzilla.redhat.com/1356949) <b>[ALL_LANG] Check for upgrades related event is not translated properly</b><br>
 - [BZ 1415759](https://bugzilla.redhat.com/1415759) <b>Trying to sparsify a direct lun via the REST API gives a NullPointerException</b><br>
 - [BZ 1408982](https://bugzilla.redhat.com/1408982) <b>Lease related tasks remain on SPM</b><br>
 - [BZ 1419529](https://bugzilla.redhat.com/1419529) <b>radio buttons overflow in Network Interface form</b><br>
 - [BZ 1418002](https://bugzilla.redhat.com/1418002) <b>[BUG] RHV cisco_ucs power management restart displays misleading message.</b><br>
 - [BZ 1413377](https://bugzilla.redhat.com/1413377) <b>Break bond and create new bond at the same time fail to get applied correctly</b><br>
 - [BZ 1410405](https://bugzilla.redhat.com/1410405) <b>unexpected TAB order in the external network subnet window</b><br>
 - [BZ 1368487](https://bugzilla.redhat.com/1368487) <b>RHGS/Gluster node is still in non-operational state, even after restarting the glusterd service from UI</b><br>
 - [BZ 1364137](https://bugzilla.redhat.com/1364137) <b>make VM template should be blocked while importing this VM.</b><br>
 - [BZ 1390575](https://bugzilla.redhat.com/1390575) <b>Import VM from data domain failed when trying to import a VM without re-assign MACs, but there is no MACs left in the destination pool</b><br>
 - [BZ 1406572](https://bugzilla.redhat.com/1406572) <b>Uncaught exception is received when trying to create a vm from User portal without power user role assigned</b><br>
 - [BZ 1421973](https://bugzilla.redhat.com/1421973) <b>[UI] Uncaught exception when dragging the whole interface panel on top of other interface panel in the Setup Networks dialog</b><br>
 - [BZ 1374589](https://bugzilla.redhat.com/1374589) <b>remove virtio-win drivers drop down for KVM imports</b><br>
 - [BZ 1422505](https://bugzilla.redhat.com/1422505) <b>metrics stuff should be in own repo</b><br>
 - [BZ 1419327](https://bugzilla.redhat.com/1419327) <b>Discard data is not supported after upgrading the DC to 4.1</b><br>
 - [BZ 1419352](https://bugzilla.redhat.com/1419352) <b>After upgrading from 4.0.6 to 4.1 the GUI dialog for moving the disks from one Storage to another is not rendered correctly (when multiple disks(>8) are selected for move.)</b><br>
 - [BZ 1408134](https://bugzilla.redhat.com/1408134) <b>BUG: "interface" field empty in "attach disk" windows Ovirt 4.0.5</b><br>
 - [BZ 1420265](https://bugzilla.redhat.com/1420265) <b>Attempting to remove disk from storage domain in template's storage sub-tab results in exception</b><br>
 - [BZ 1415806](https://bugzilla.redhat.com/1415806) <b>oVirt 4.1 translation cycle 2</b><br>
 - [BZ 1425705](https://bugzilla.redhat.com/1425705) <b>taskcleaner.sh fails on non existing columns in command_entities table</b><br>
 - [BZ 1379225](https://bugzilla.redhat.com/1379225) <b>cannot assign gluster network role using api</b><br>
 - [BZ 1412626](https://bugzilla.redhat.com/1412626) <b>[scale] High Database Load after updating to oVirt 4.0.4 (select * from  getdisksvmguid)</b><br>
 - [BZ 1419520](https://bugzilla.redhat.com/1419520) <b>Detaching all vms from a pool via REST API doesn't remove the pool</b><br>
 - [BZ 1420302](https://bugzilla.redhat.com/1420302) <b>Adding fence agent with type "invalid_type" succeeds</b><br>
 - [BZ 1422562](https://bugzilla.redhat.com/1422562) <b>Return value of engine-vacuum on successful should be always 0</b><br>
 - [BZ 1421942](https://bugzilla.redhat.com/1421942) <b>Update dashboards DWH views version and dashboards queries</b><br>
 - [BZ 1421962](https://bugzilla.redhat.com/1421962) <b>[RFE] Add JMX support for jconsole</b><br>
 - [BZ 1416830](https://bugzilla.redhat.com/1416830) <b>Search by tags generates wrong filter string in Users tab</b><br>
 - [BZ 1414430](https://bugzilla.redhat.com/1414430) <b>Disable sparsify option for pre allocated disk</b><br>
 - [BZ 1390271](https://bugzilla.redhat.com/1390271) <b>in few of ui dialogs the fields position is pushed down or cut after replacing to the new list boxes</b><br>
 - [BZ 1422089](https://bugzilla.redhat.com/1422089) <b>Missing exit code for post-copy migration failure</b><br>
 - [BZ 1399603](https://bugzilla.redhat.com/1399603) <b>Import template from glance and export it to export domain will cause that it is impossible to import it</b><br>
 - [BZ 1417439](https://bugzilla.redhat.com/1417439) <b>When adding lease using REST high availability should be enabled first</b><br>
 - [BZ 1421285](https://bugzilla.redhat.com/1421285) <b>oVirt 4.1 update it_IT community translation</b><br>
 - [BZ 1421619](https://bugzilla.redhat.com/1421619) <b>Command proceed to perform the next execution phase although execute() failed</b><br>
 - [BZ 1417903](https://bugzilla.redhat.com/1417903) <b>Trying to download an image when it's storage domain is in maintenance locks the image for good</b><br>
 - [BZ 1420821](https://bugzilla.redhat.com/1420821) <b>style issues in block storage dialog</b><br>
 - [BZ 1420816](https://bugzilla.redhat.com/1420816) <b>missing Interface column on Storage and Disks sub-tabs under Templates main-tab</b><br>
 - [BZ 1420812](https://bugzilla.redhat.com/1420812) <b>DirectLUN dialog - missing label for 'Use Host' select-box</b><br>
 - [BZ 1419853](https://bugzilla.redhat.com/1419853) <b>Image upload fails when one of the ovirt-imageio-daemons was not running</b><br>
 - [BZ 1419886](https://bugzilla.redhat.com/1419886) <b>Upload image operations are available using the GUI when there is an active download of the image using the python sdk (for the image that is downloaded)</b><br>
 - [BZ 1412687](https://bugzilla.redhat.com/1412687) <b>Awkward attempted login error</b><br>
 - [BZ 1419337](https://bugzilla.redhat.com/1419337) <b>Random Generator setting did not saved after reboot</b><br>Previously, if the RNG configuration was changed on a running VM, after restart of the VM the configuration was not properly restored.<br><br>Please note that the fix takes effect only on VMs which have been restarted on the fixed version of the engine.
 - [BZ 1414126](https://bugzilla.redhat.com/1414126) <b>UI error on 'wipe' and 'discard' being mutually exclusive is unclear and appears too late</b><br>
 - [BZ 1416845](https://bugzilla.redhat.com/1416845) <b>Can not add power management to the host, when the host has state 'UP'</b><br>
 - [BZ 1379130](https://bugzilla.redhat.com/1379130) <b>Unexpected client exception when glance server is not reachable</b><br>
 - [BZ 1419364](https://bugzilla.redhat.com/1419364) <b>Fail to register an unregistered Template through REST due to an NPE when calling updateMaxMemorySize</b><br>
 - [BZ 1414083](https://bugzilla.redhat.com/1414083) <b>User Name required for login on behalf</b><br>
 - [BZ 1414086](https://bugzilla.redhat.com/1414086) <b>Remove redundant video cards when no graphics available for a VM and also add video cards if one graphics device exists</b><br>
 - [BZ 1347356](https://bugzilla.redhat.com/1347356) <b>Pending Virtual Machine Changes -> minAllocatedMem issues</b><br>
 - [BZ 1416340](https://bugzilla.redhat.com/1416340) <b>Change name of check box in Edit Virtual Disks window from Pass Discard to Enable Discard</b><br>
 - [BZ 1400500](https://bugzilla.redhat.com/1400500) <b>If AvailableUpdatesFinder finds already running process it should not be ERROR level</b><br>
 - [BZ 1416837](https://bugzilla.redhat.com/1416837) <b>Order VMs by Uptime doesn't work</b><br>
 - [BZ 1416809](https://bugzilla.redhat.com/1416809) <b>New HSM infrastructure - No QCow version displayed for images created with 4.0</b><br>
 - [BZ 1416147](https://bugzilla.redhat.com/1416147) <b>Version 3 of the API doesn't implement the 'testconnectivity' action of external providers</b><br>
 - [BZ 1415639](https://bugzilla.redhat.com/1415639) <b>Some of the values are not considered as numbers by elasticsearch</b><br>
 - [BZ 1414084](https://bugzilla.redhat.com/1414084) <b>uploaded images using the GUI have actual_size=0</b><br>

#### VDSM

 - [BZ 1426727](https://bugzilla.redhat.com/1426727) <b>VM dies during live migration with CPU quotas enabled</b><br>
 - [BZ 1374545](https://bugzilla.redhat.com/1374545) <b>Guest LVs created in ovirt raw volumes are auto activated on the hypervisor in RHEL 7</b><br>
 - [BZ 1414323](https://bugzilla.redhat.com/1414323) <b>Failed to add host to engine via bond+vlan configured by NM during anaconda</b><br>
 - [BZ 1368364](https://bugzilla.redhat.com/1368364) <b>Reported Node version and release are incorrect - RHEL version should be reported</b><br>
 - [BZ 1419931](https://bugzilla.redhat.com/1419931) <b>Failed to destroy partially-initialized VM with port mirroring</b><br>
 - [BZ 1408190](https://bugzilla.redhat.com/1408190) <b>[RFE] Update timestamp format in vdsm log to timestamp with timezone</b><br>
 - [BZ 1412563](https://bugzilla.redhat.com/1412563) <b>parse arp_ip_target with multiple ip properly</b><br>
 - [BZ 1410076](https://bugzilla.redhat.com/1410076) <b>[SR-IOV] - in-guest bond with virtio+passthrough slave lose connectivity after hotunplug/hotplug of passthrough slave</b><br>
 - [BZ 1427444](https://bugzilla.redhat.com/1427444) <b>post copy event handling triggered by any libvirt event</b><br>
 - [BZ 1425233](https://bugzilla.redhat.com/1425233) <b>Ensure that the HE 3.5 -> 3.6 upgrade procedure is still working if executed on 4.1 host with jsonrpc</b><br>
 - [BZ 1412583](https://bugzilla.redhat.com/1412583) <b>Multiple disconnect messages in VDSM log</b><br>
 - [BZ 1376116](https://bugzilla.redhat.com/1376116) <b>Vdsm install "api" package in the python global namespace</b><br>
 - [BZ 1422087](https://bugzilla.redhat.com/1422087) <b>wrong VMware OVA import capacity</b><br>
 - [BZ 1421556](https://bugzilla.redhat.com/1421556) <b>Setting log level as shown in README.logging fails.</b><br>
 - [BZ 1412550](https://bugzilla.redhat.com/1412550) <b>Fix certificate validation for engine <-> VDSM encrypted connection when IPv6 is configured</b><br>
 - [BZ 1414626](https://bugzilla.redhat.com/1414626) <b>Crash VM during migrating with error "Failed in MigrateBrokerVDS"</b><br>
 - [BZ 1419557](https://bugzilla.redhat.com/1419557) <b>Switching to post-copy should catch exceptions</b><br>
 - [BZ 1417460](https://bugzilla.redhat.com/1417460) <b>Failed to Amend Qcow volume on block SD due failure on Qemu-image</b><br>
 - [BZ 1417737](https://bugzilla.redhat.com/1417737) <b>Cold Merge: Deprecate mergeSnapshots verb</b><br>
 - [BZ 1415803](https://bugzilla.redhat.com/1415803) <b>Improve logging during live merge</b><br>

#### oVirt Engine Extension AAA JDBC

 - [BZ 1415704](https://bugzilla.redhat.com/1415704) <b>Casting exception during group show by ovirt-aaa-jdbc tool</b><br>

#### oVirt Hosted Engine HA

 - [BZ 1425233](https://bugzilla.redhat.com/1425233) <b>Ensure that the HE 3.5 -> 3.6 upgrade procedure is still working if executed on 4.1 host with jsonrpc</b><br>

#### oVirt Release Package

 - [BZ 1418630](https://bugzilla.redhat.com/1418630) <b>gluster firewalld service should be added to the default firewall zone</b><br>
 - [BZ 1419105](https://bugzilla.redhat.com/1419105) <b>oVirt Node NG does not include vdsm-hook-vhostmd</b><br>
 - [BZ 1411640](https://bugzilla.redhat.com/1411640) <b>[HC] - Include gdeploy package in oVirt Node</b><br>

#### oVirt Engine Extension AAA LDAP

 - [BZ 1420281](https://bugzilla.redhat.com/1420281) <b>Ignore groups which can't be resolved from non-working domain inside Active Directory multi-domain forrest</b><br>
 - [BZ 1420745](https://bugzilla.redhat.com/1420745) <b>[aaa-ldap-setup] No validation on profile names when using config file</b><br>
 - [BZ 1408678](https://bugzilla.redhat.com/1408678) <b>[aaa-ldap-setup] Duplicate profile names definitions on availableProfiles</b><br>

#### imgbased

 - [BZ 1394259](https://bugzilla.redhat.com/1394259) <b>RHV-H 4.0 grub2-mkconfig fails</b><br>
 - [BZ 1421699](https://bugzilla.redhat.com/1421699) <b>imgbase base --of-layer fails with exception when base is provided.</b><br>
 - [BZ 1366549](https://bugzilla.redhat.com/1366549) <b>imgbase rollback does not result in a rollback (no default boot entry change)</b><br>
 - [BZ 1415026](https://bugzilla.redhat.com/1415026) <b>Both imgbase status and node status are shown on the screen after upgrade</b><br>
 - [BZ 1417100](https://bugzilla.redhat.com/1417100) <b>imgbased make distcheck fails</b><br>
 - [BZ 1426172](https://bugzilla.redhat.com/1426172) <b>RHVH new build boot entry miss when upgrade from wrapper to wrapper</b><br>

#### oVirt Host Deploy

 - [BZ 1414265](https://bugzilla.redhat.com/1414265) <b>config.py:_validation has two decorations</b><br>
 - [BZ 1411491](https://bugzilla.redhat.com/1411491) <b>Disk image upload fails on self-hosted engine, requires imageio-daemon restart</b><br>

#### oVirt Engine Dashboard

 - [BZ 1421095](https://bugzilla.redhat.com/1421095) <b>[fr_FR] [Admin Portal] Need to change a string on Dashboard tab (corrected in zanata)</b><br>
 - [BZ 1365929](https://bugzilla.redhat.com/1365929) <b>tooltips in dashboard Utilization boxes move on page scroll</b><br>
 - [BZ 1369550](https://bugzilla.redhat.com/1369550) <b>ovirt-engine dashboard - set status cards to same height when lines wrap</b><br>
 - [BZ 1362402](https://bugzilla.redhat.com/1362402) <b>dashboard: make the date format of time-series graph in locale specific formats in the UTC time zone</b><br>
 - [BZ 1392984](https://bugzilla.redhat.com/1392984) <b>Status card content should not wrap</b><br>
 - [BZ 1415241](https://bugzilla.redhat.com/1415241) <b>oVirt 4.1 translation cycle 1</b><br>

#### oVirt Engine SDK 4 Python

 - [BZ 1425731](https://bugzilla.redhat.com/1425731) <b>APIv4: StorageDomainVmService.register should have reassign_bad_macs, not import_</b><br>
 - [BZ 1422979](https://bugzilla.redhat.com/1422979) <b>tarball packaging issues -> pypi different data</b><br>

#### oVirt Engine SDK 4 Ruby

 - [BZ 1428642](https://bugzilla.redhat.com/1428642) <b>version 4.1.3 - uninitialized constant OvirtSDK4::MigrationOptionsReader::AutoConverge</b><br>

## Bug fixes

### oVirt Cockpit Plugin

 - [BZ 1415651](https://bugzilla.redhat.com/1415651) <b>provide an easy way for the user to redeploy in case deployment fails</b><br>
 - [BZ 1415195](https://bugzilla.redhat.com/1415195) <b>change 'yum update' label to 'Update Host' and remove gpgcheck checkbox</b><br>
 - [BZ 1415665](https://bugzilla.redhat.com/1415665) <b>Gdeploy setup window closes if focus outside modal dialog</b><br>
 - [BZ 1415989](https://bugzilla.redhat.com/1415989) <b>engine volume brick should be created with thick LV</b><br>
 - [BZ 1415189](https://bugzilla.redhat.com/1415189) <b>Give a valid message to the user when gdeploy is not installed in the system.</b><br>
 - [BZ 1426494](https://bugzilla.redhat.com/1426494) <b>Remove 'poolmetadatasize' from gdeploy config file, so that gdeploy can manipulate the same</b><br>
 - [BZ 1427103](https://bugzilla.redhat.com/1427103) <b>Registering to CDN credentials are missing with cockpit UI</b><br>

### oVirt Engine

 - [BZ 1417518](https://bugzilla.redhat.com/1417518) <b>[HE] high availability compromised due to duplicate spm id</b><br>
 - [BZ 1424813](https://bugzilla.redhat.com/1424813) <b>[UI] - Can't make any changes in custom mode field after pressed on 'OK' button one time</b><br>
 - [BZ 1416459](https://bugzilla.redhat.com/1416459) <b>Restore HE backup will fail if the HE SD has disks of non-HE VM's</b><br>
 - [BZ 1382807](https://bugzilla.redhat.com/1382807) <b>[UX] NUMA pinning dialog should prevent unsupported layout</b><br>
 - [BZ 1419924](https://bugzilla.redhat.com/1419924) <b>cluster level 4.1 adds Random Generator to all VMs while it may not be presented by cluster</b><br>
 - [BZ 1421713](https://bugzilla.redhat.com/1421713) <b>VM pinned to host is started on another host</b><br>
 - [BZ 1406243](https://bugzilla.redhat.com/1406243) <b>Out of range CPU APIC ID</b><br>
 - [BZ 1388963](https://bugzilla.redhat.com/1388963) <b>Unable to change vm Pool Configuration. Receive "Uncaught exception occurred. Please try reloading the page".</b><br>
 - [BZ 1416893](https://bugzilla.redhat.com/1416893) <b>Unable to undeploy hosted-engine host via UI.</b><br>
 - [BZ 1364132](https://bugzilla.redhat.com/1364132) <b>Once the engine imports the hosted-engine VM we loose the console device</b><br>
 - [BZ 1317490](https://bugzilla.redhat.com/1317490) <b>[engine-backend] Disks are alphabetically ordered instead of numerically, which causes the guest to see them this way (1,10,2..) instead of (1,2,10)</b><br>
 - [BZ 1401963](https://bugzilla.redhat.com/1401963) <b>installed webadmin-portal-debuginfo is not updated by engine-setup and brokes the engine</b><br>
 - [BZ 1416748](https://bugzilla.redhat.com/1416748) <b>punch iptables holes on OVN hosts during installation</b><br>
 - [BZ 1276670](https://bugzilla.redhat.com/1276670) <b>[engine-clean] engine-cleanup doesn't stop ovirt-vmconsole-proxy-sshd</b><br>
 - [BZ 1329893](https://bugzilla.redhat.com/1329893) <b>UI: explain why we cannot change the logical network settings in the "Manage Networks" window</b><br>
 - [BZ 1416466](https://bugzilla.redhat.com/1416466) <b>Restore HE backup will fail if the HE host has running non-HE VM's</b><br>
 - [BZ 1422374](https://bugzilla.redhat.com/1422374) <b>The 'VirtIO-SCSI Enabled' isn't enabled by default for new templates</b><br>
 - [BZ 1417935](https://bugzilla.redhat.com/1417935) <b>VM to host affinity :  conflict detection mechanism</b><br>
 - [BZ 1411844](https://bugzilla.redhat.com/1411844) <b>Imported VMs have maxMemory too large</b><br>
 - [BZ 1273825](https://bugzilla.redhat.com/1273825) <b>Template sorting by version is broken</b><br>
 - [BZ 1394687](https://bugzilla.redhat.com/1394687) <b>DC gets non-responding when detaching inactive ISO domain</b><br>
 - [BZ 1323663](https://bugzilla.redhat.com/1323663) <b>the path of storage domain is not trimmed/missing warning about invalid path</b><br>
 - [BZ 1417458](https://bugzilla.redhat.com/1417458) <b>Cold Merge: Use volume generation</b><br>

### VDSM

 - [BZ 1341106](https://bugzilla.redhat.com/1341106) <b>HA vms do not start after successful power-management.</b><br>
 - [BZ 1302020](https://bugzilla.redhat.com/1302020) <b>[Host QoS] - Set maximum link share('ls') value for all classes on the default class</b><br>
 - [BZ 1223538](https://bugzilla.redhat.com/1223538) <b>VDSM reports "lvm vgs failed" warning when DC contains ISO domain</b><br>
 - [BZ 1403846](https://bugzilla.redhat.com/1403846) <b>keep 3.6 in the supportedEngines reported by VDSM</b><br>
 - [BZ 1302358](https://bugzilla.redhat.com/1302358) <b>File Storage domain export path does not support [IPv6]:/path input</b><br>

### oVirt Engine Extension AAA LDAP

 - [BZ 1409827](https://bugzilla.redhat.com/1409827) <b>[RFE] Add documentation how to remove LDAP provider configuration</b><br>

### imgbased

 - [BZ 1417534](https://bugzilla.redhat.com/1417534) <b>unmodified configuration files should be updated during update.</b><br>
 - [BZ 1333742](https://bugzilla.redhat.com/1333742) <b>imgbased --version info is not consistent with imgbased rpm version</b><br>
 - [BZ 1419535](https://bugzilla.redhat.com/1419535) <b>Garbage collection of layers doesn't work</b><br>

### oVirt Host Deploy

 - [BZ 1412906](https://bugzilla.redhat.com/1412906) <b>ovirt-engine can't install legacy RHV-H in 3.6 Compatibility Mode</b><br>
 - [BZ 1381219](https://bugzilla.redhat.com/1381219) <b>Remove udev rule for deadline elevator (from host deploy) - not needed in EL7 hosts.</b><br>

### oVirt Provider OVN

 - [BZ 1416748](https://bugzilla.redhat.com/1416748) <b>punch iptables holes on OVN hosts during installation</b><br>

