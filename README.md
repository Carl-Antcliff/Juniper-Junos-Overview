## Product portfolio
### Routers
- M Series - first product by Juniper Networks, multiservice router (routing and MPLS services, no switches) Designed for enterprise and service providers.
- T Series - no switching, only routing, old, performance upgrade for M. High end and core routers.
- MX Series - universal routing platform used for routing and also switching. Featured a Trio chipset. High throughput.
- J series - used for small offices and have integrated security (similar to SRX but older).
- PTX series - packet transfer routers. High bandwidth, for core, Tbps throughput. Optimized for core and data centers.
- ACX Series - Universal Access Router - optimized for metro network architecture, access aggregation, LTE deployments, mobile backhaul.
- SRX - Service Gateway devices. Firewall, switch, WiFi, 3G, VoIP, Routing. All in one platform.

### Switches
- EX - L3 switches
- QFX - data center switches
- OCX - recent switches, focused on open, disaggregated networking systems

### SDN
- NFX250 - network services platform. Used as CPE. Virtualized security appliance with native SRX Series software.
- NorthStart Controller - SND controller for traffic engineering (optimization). It automates the control of segment routing and IP/MPLS flows.
- IP/MPLSView is a multivendor, multiprotocol, and multilayer operations support systems (OSS) traffic management and traffic engineering solution for IP and/or MPLS networks
- Contrail - SDN controller for network management and orchestration from the cloud.

## Juniper abbreviations and terminology
- Control plane - The control plane is the router's brain and its computational element. Usually implemented on a Routing Engine (RE)
- RE - Routing engine - Responsible for OSPF, SSH, etc... Has a kernel, it is an x86 with flash, SSD and RAM. Processes exception traffic (routing protocol updates, management traffic for the RE an packets with IP options). Is connected to the line cards with an internal ethernet switch (`show chassis ethernet switch`). The internal link has a built-in rate limiter that is not configurable and protects against DoS attacks.
- FP - Forwarding Plane. It is a set of linecards interconnected by a switch fabric, that together represent the forwarding plane (also called the data plane).
- PFE - Packet Forwarding Engine, the intelligent component of the forwarding plane. One line card can have multiple PFE. Runs on ASICs for increased performance. Uses Layer 2 and Layer 3 forwarding tables to forward traffic toward its destination. Implements various services such as policing, stateless firewall filtering, and class of service.
- SCB - Switch Control Board. It has three primary functions: switch data between the line cards, control the chassis, and house the routing engine (RE is usually inserted into this card).
- RT - Routing table. Contains all the valid routes. It is located in the RE and updated by routing protocols. Also referred as RIB - Routing information base. 
- FT - Forwarding table. Contains all the active (best) routes from the RT. It is located and used by PFE. Also referred as FIB - Forwarding information base. The RE is responsible of updating this table in all installed PFE.
- Route Preference is also known as an _administrative distance_ in Cisco.
- Metric is used to select best route when the preference is the same, i.e.: when both routes are learned via the same protocol they will have same preference but they may have different metric.
- FPC - Flexible PIC Concentrator. Referred as linecard (_ge-1/0/0_, _ge-2/0/0_).
- PIC - Physical Interface Card. Logical port separation on the line card (_ge-3/0/0_, _ge-3/1/0_).
- MIC - Modular Interface Card. MICs provide the physical connections to various network media types. MICs allow different physical interfaces to be supported on a single line card. An FPC port can be separated into few parts allowing to plug multiple PICs into it, one MIC could contain ethernet ports and another ATM ports.
- Interface ge-0/2/3 = physical Gigabit Ethernet port 3 of a Gigabit Ethernet PIC in slot 2 on FPC 0
- Line card types:  
  - DPC - Dense Port Concentrator.
  - DPCE - Dense Port Concentrator Enhanced card.
  - MPC - Modular Port Concentrator. Inserted into FPC. Second-generation high-density and high-speed line cards .
  - MS-MIC/MPC - Multiservice MIC/MPC. Can offer services as PAT and IPSec. Normal line cards some times does not have those services so a separate card is required.
  - Different plattforms use different names for line cards and interface cards, but the CLI almos always refers to them as FPC and PIC.
- Transient interface - Interface that belong to a MIC. They are referred to as transient because you can hot-swap a line card or MIC card at any time.
- PEM - Power Entry Module (Power supply)

### Junos versions
```
19.1 R 2.1
 ^ ^ ^ ^ ^
 | | | | + spin number
 | | | + build number
 | | + software release
 | + minor release number 
 + main release number
 ```

Release types:
X - Security focused release (special eXception).
R - Maintenance release. Released about every eight weeks to fix a collection of issues. No features added.
S - Service release. Released on demand to fix critical issues that will be addressed by mantenance release.
F - Feature velocity release. Introduce innovative features that later on will be added to a next minor release.
 
### FreeBSD terminology
- IFD — interface device — physical port on the device (_xe-0/0/0_)
- IFL — interface logical — number of the logical unit (_unit 0_)
- IFF — interface family (_family inet_)
- IFA — interface address — network address (_10.0.0.1_)

### FreeBSD daemons
- DCD - Device Configuration Daemon. Controls both the physical and logical properties of the
interfaces.
- PPMD - Periodic packet manager. Responsible of processing protocol keepalives, hellos, etc...
- RPD - Routing Protocol Daemon. Its functionality includes all protocol messages, routing table updates, and implementation of routing policies. Restart using process `restart routing`
- MGD - Management Daemon.  Controls all user access to the router. For example, the user’s CLI is a client of _mgd_.
- CHASSISD - Chassis Daemon. Controls the properties of the router itself, including the interaction of the passive midplane, the FPCs, and the control boards.
- PFED - Packet Forwarding Engine Daemon. Controls the communication between the Packet Forwarding Engine and the Routing Engine. 

## CLI user interface

### Keyboard shortcuts

| Sequence | Action
|----------|----------------------------------------------------------------------------
| Alt + b  | Move the cursor back one word
| Alt + f  | Move the cursor forward one word
| Ctrl + a | Move the cursor to the beginning of the command line
| Ctrl + e | Move the cursor to the end of the command line
| Ctrl + d | Delete the character at the cursor
| Ctrl + k | Delete the all characters from the cursor to the end of the command line
| Ctrl + u | Delete the all characters from the command line
| Ctrl + w | Delete the word before the cursor
| Alt + d  | Delete the word after the cursor

### CLI settings
`set cli screen-length <length>` number of lines of text that the terminal screen displays. The point at which the _---(more)---_ prompt appears on the screen is a function of this setting. Executed from operational mode.

### Configuration modes
- `configure` or `edit`   - If you and another user make changes and another user commits it's changes while you are editing, your changes are committed as well.
- `configure private`   - If the configuration has been modified by another user, you can merge the modifications into your private candidate configuration and attempt to commit again.
- `configure exclusive` - Configure exclusive only allows one user to be making changes at a time.

### Configuration mode commands
- `delete` delete all the contents from the current config block
- `up` go up one level
- `up <x>` move up the specified number of levels in the configuration hierarchy.
- `up <number> <configuration-command>`  issue configuration mode commands from a location higher in the hierarchy.
- `exit` exit configuration mode from the top level
- `exit configuration-mode` exit configuration from any level
- `top` go to the top of configuration
- `top show` top can be combined with other commands
- `copy existing-statement to new-statement` make a copy of an existing statement in the configuration.
- `replace pattern <pattern> with <new_value>` a pattern inside of a config block can be replaced. This is similar to 'find and replace' used in text editors. Very helpful when ordering terms in - filters`insert term 2 before term 1` at the [edit policy-options policy-statement XXXXXX] hierarchy level to change statement order after the term is created.
- `wildcard delete <statement-path> <identifier> <regular-expression>` Delete a statement or identifier. `wildcard delete interface ge-0/0/*` deletes all the interfaces starting with _ge-0/0/*_
- `deactivate interfaces ge-0/0/0` Deactivates the configuration of the specified block
- `set interfaces ge-0/0/0 disable` Shut down the physical interface, `deactivate` will only deactivate config, not the physical interface.
- `annotate <statement> <comment string>` Add comments to a configuration.

### Viewing and comparing the Configuration
- `show | display set` display the configuration as a series of configuration mode commands required to re-create the configuration from the top level of the hierarchy as _set_ commands.
- `show | display set relative` display configuration relative to the current hierarchy level.
- `show | display inheritance` display configuration with config groups applied.
- `show | display xml` display config in xml/netconf format.
- `show | display detail` display command description and usage
- `show | display inheritance` include inherited configuration
- `show | display omit` include omitted blocks
- `show | except <regexp>` show only text that does not match a pattern
- `show | find <regexp>` search for first occurrence of pattern
- `show | match <regexp>` show only text that matches a pattern
- `show | hold` hold text without exiting the _--More--_ prompt
- `show | no-more` displays all content at once without pagination
- `show | last` display end of output only
- `show | resolve` resolve IP addresses
- `show | save` save output text to file
- `show | tee` write to standard output and file
- `show | trim <n-columns>` trim specified number of columns from start of line
- `show | compare` compare running configuration with the candidate

### Load and save configuration using terminal (from the current hierarchy)
In configuration mode:
- `save terminal` print the current config to terminal
- `load merge|override|replace|update|patch terminal [relative]` load the config (with braces and hierarchy) from terminal. `override` deletes the entire config and replaces it with a new one while `replace` only replaces the existing statements defined in the loaded config.
- `save <filename>` save configuration to file. If no path name specified, it will save to _/var/home/username_
- `save [ftp://|scp://]` configuration could be saved to a network resource
- `load factory-default` load factory default configuration

### Load configuration from file
- `load merge 'filename'` load configuration from filename. Partial configuration should be loaded from the right config hierarchy level
- `load merge 'filename' relative` let the system detect the right level to insert the changes
- `load override 'filename'` replace the configuration with the configuration from the file

### Saving changes
Edit mode creates a _candidate_ configuration which will become active after issuing a `commit` command.
- `commit` commit current changes.
- `commit and-quit` commit and return to privileged mode.
- `commit check` checks but does not applies the candidate config.
- `commit confirmed X` applies the candidate config but for a specified amount of minutes (10 by default). If there is no `commit` issued after X minutes, the applied configuration is reverted.
- `commit comment "changed bla bla bla"` add a comment to the commit.
- `commit at time` configuration will be scheduled to commit at specified time.
- `clear system commit` remove the scheduled commit.
- `show system commit` shows scheduled system commits.
- `commit synchronize` commit and sync the config on both RE. `set system commit synchornize` forces to synchronize the config on each commit automatically.

### Rollback
- `show system commit` show information about all previous commits with the commit messages
- `rollback 1` overwrite the candidate config with the previous one. Won't apply until `commit` is issued
- `rollback 0` or `rollback` reset all changes made by candidate config. Won't apply until `commit` is issued
- `set max-configurations-on-flash <x>` specify the amount of config versions to save (defaults to 50)

### Comparing configuration
-  From privileged mode
   - `show system rollback 3 compare 1` compare third rollback configuration with the first one
   - `show configuration | compare rollback x` compare current configuration with a rollback configuration
-  From configuration mode
    - `show | compare rollback 3` compare candidate configuration with a rollback configuration

### Rescue configuration
- `request system configuration rescue save` save current configuration as rescue
- `request system configuration rescue delete` delete current rescue configuration
- `show system configuration rescue` show rescue configuration
- `rollback rescue` revert changes to the recue config
- `file show /config/rescue.conf.gz` show the file which contains the rescue configuration

### Using CLI help
- `help topic routing-options static`  show command usage guidelines (very detailed)
- `help reference routing-options static` show command summary information (syntax, default parameters and allowed value ranges, the configuration level at which the command can be entered)
- `help apropos <text>`  find a list of all configuration-mode commands that contain a particular text string
- `help syslog <MESSAGE_CODE>` message code documentation

### Working with files
- `file list <dir>` show file list in specified directory
- `file show <filename>` show contents of a file
- `file compare files <file1> <file2>` compare two files
- `file delete <filename>`

### Shell
- `start shell` go to Free BSD cli
- `start shell pfe network fpc0` go to shell of the line card management console

## Configuration Basics

### Factory reset

#### Reset configuration only
```
configure
load factory-default /* replace current config with the factory default config */
set system root-authentication plain-text-password /* Junos will not let you commit without setting the root password */
commit and-quit
```

#### Full factory reset
`request system zeroize` hard factory reset, deletes logs and some files

### User accounts and login classes
#### Authentication methods
`set system authentication-order [radius password]` password method will be used if radius denies access. If we don't specify "password", we will only fallback to password if radius is not available (we won't fallback if radius rejects the credentials)

#### Radius server configuration
```
system radius server 1.1.1.1 {
  secret xxx
}
```
#### Permission control granularity
```
+User - defines authorization parameters
|--+Class - named container that groups permission flags
   |--+Permission flags - predefined sets of related commands
      |--Deny and Allow overrides - define exceptions for commands and config statements
```
Junos predefined login classes
| Login Class                  | Permission Flag Set                     |
|------------------------------|-----------------------------------------|
| `operator`                   | clear, network, reset, trace, and view  |
| `readonly`                   | view                                    |
| `superuser` or `super-user`  | all                                     |
| `unauthorized`               | None                                    | 

#### Creating users
Create user _Bla_ with a _super-user_ class
```
configure
edit system login
set user Bla class super-user
set user Bla authentication plain-text-password <xxxx>
commit and-quit
```

#### Changing custom classes
**Note:** deny statements are checked before allow statements.

```
edit system login
set class CustomClass permissions [...] /* create custom user class with specified permission flags */
set class CustomClass deny-commmands "(regular-expression)|(regular-expression1)..." /* explicitly deny specified operational mode commands */
set class CustomClass allow-commmands "(regular-expression)|(regular-expression1)... /* explicitly allow specified operational mode commands */
set class CustomClass deny-configuration "regular-expression)|(regular-expression1)..." /* explicitly deny configuration access to the specified levels in the hierarchy */
set class CustomClass allow-configuration "interfaces|firewall" /* explicitly allow configuration access to the specified levels in the hierarchy */
```

#### Changing user home directory
From operational mode:
```
set cli directory <dir>
```
List user files:
`file list /var/home/Bla`

#### Configuration groups
```
configure
edit groups
all-atm {
  interfaces {
    <at-*> {
      encapsulation atm-pvc;
      atm-options {
        ...
      }
      unit 100 {
        point-to-pint;
        ...
      }
    }
  }
}
top
edit interfaces
apply-groups all-atm;
commit and-quit
```
Verification:
- `show interfaces at-0/0/1 | display inheritance` shows the config with all the applied configuration from groups and also comments of which element was inherited from which apply group
- `show interfaces at-0/0/1 | display inheritance except #` exclude comments related to inherited statements

### Logging
Each system log message belongs to a facility, which groups together messages that either are generated by the same source (such as a software process) or concern a similar condition or activity (such as authentication attempts). Each message is also preassigned a _severity level_, which indicates how seriously the triggering event affects routing platform functions.

Configuration:
```
configure
edit system syslog
set user Bla any any /* Send any type of message of any type of severity to user's console. */
set host <x.x.x.x> 'facility' 'severity' /* Send messages to a syslog server */
set file messages 'facility' 'severity' /* Save messages in the 'messages' log file */
set console 'facility' 'severity' /* Send messages to the system console */
commit and-quit
```
The Junos default configuration includes the `interactive-commands` and  `messages` files configuration.
The `interactive-commands` log file stores the `interactive-commands` facility events (commands issued at the Junos command-line or by a client application). The `messages` log file saves events that occurs in the system (routine operations, failures and errors, emergency and critical conditions).

**NOTE:** `change-log` facility allows to log changes to the Junos configuration. `interactive-commands` logs only priviledged mode commands.

Verification:
- `help syslog <MESSAGE_CODE>` message code documentation
- `show log messages`  show the contents of the _messages_ log file
- `show log messages | match ospf` filters _messages_ log file and shows everything containing ospf
- `show log messages | match 'ospf|bgp'`  filters _messages_ log file and shows everything containing _ospf_ or _bgp_
- `clear log messages` delete the contents of _messages_ log file

System log message severity levels:
```
7 ANY
6 INFO
5 NOTICE
4 WARNINGS
3 ERRORS
2 CRITICAL
1 ALERTS
0 EMERGENCY
- NONE
```
### Tracing (debugging)
```
configure
set protocols ospf traceoptions file OSPF_SAMPLE size 128K files 10 no-world-readable replace /* the OSPF_SAMPLE file will be saved in /var/log, 128K is default size */
set protocols ospf traceoptions flag error detail
set system tracing destination-override syslog host <x.x.x.x> /* send traces to syslog server instead of saving to file */
commit and-quit
```
#### Viewing trace file
- `show log OSPF_SAMPLE`
- `clear log OSPF_SAMPLE`

#### Monitoring trace file
- `monitor start OSFP_SAMPLE` file contents will be shown on console when file is updated
- `monitor stop OSPF_SAMPLE` or press `ESC + Q` to pause monitor
- `monitor list` show currently monitored log files

**NOTE:**  interface tracing is done by the kernel and no trace file can be specified, traces are logged to _messages_ file. Global interface tracing supports archive file but by default is in _/var/log/dcd_ file

### NTP configuration
If 2 machines have > 128ms diffs, the clocks are synchronized step by step. If time diff is > 1000s no sync performed

Configuration:
```
configure
edit system ntp
set boot-server /* set up time from this server during boot */
set server /* set params of the NTP server to get the details from. This device also will act as an NTP server! */
commit and-quit
```
Verification:
- `set date ntp force` force to set date from NTP server
- `set date YYYYMMDDhhmm.ss` manually set date
- `show ntp status`
- `show ntp associations`
- `show system uptime` also shows current time

### SNMP v2
```
configure
edit snmp
set name "GW01"
set description "GW01"
set location "Lab 4 Row 11"
set contact "qfabric-admin@qfabric0"
set community public authorization read-only
set community public clients x.x.x.x/yy <restrict> /* authorized or not authorized clients */
set community public clietn-list-name <name> /* Allows to specify a list of authorized clients */
set client-list <name> <ip-address 1>
set client-list <name> <ip-address 2>
commit and-quit
```
SNMP Traps
```
[edit snmp]
trap-group MY_TRAPS {
  version v2
  categories {
    chassis
    link
    routing
  }
  targets {
    1.1.1.1
  }
}
```
`show snmp mib walk jnxOpertingDescr` show MIB values

### Automatic backup
```
config
edit system
set archival configuration transfer-on-commit
set archival configuration transfer-interval 1440 /* The interval is a period of time ranging from 15 through 2880 minutes */
set archival configuration archive-sites "scp://..."
set archival configuration archive-sites "ftp://..." /* Will be used if first site fails */
```
Backup log is stored in _/var/log/transfer/config_ file

### Interfaces configuration
- Logical units are always required. 
- Logical unit 0 is the only value allowed on interfaces running HDLC or PPP encapsulations.

Interface naming conventions:
| Name | Description |
|------|-------------|
|`ae`  | Aggregated Ethernet interface
|`at`  | ATM interface
|`em`  | Management and internal Ethernet interfaces (same as `fxp`, only one type will be present)
|`et`  | 100-Gigabit Ethernet interfaces (also could be used for 40-Gigabit Ethernet interfaces)
|`fe`  | Fast Ethernet interface
|`fxp` | Management and internal Ethernet interfaces
|`ge`  | Gigabit Ethernet interface
|`gr`  | Generic routing encapsulation (GRE) tunnel interface
|`lo`  | Loopback interface. The Junos OS automatically configures one loopback interface (lo0).
|`si`  | Services-inline interface, which is hosted on a Trio-based line card.
|`so`  | SONET/SDH interface
|`xe`  | 10-Gigabit Ethernet interface. Some older 10-Gigabit Ethernet interfaces use the `ge` media type (rather than `xe`) to identify the physical part of the network device.

Single interface configuration
```
configure
edit interfaces ge-0/0/1 /* set physical parameters like speed, duplex, etc... under this stanza */
set link-mode full-duplex
set speed 1000
set mtu 1500
set mac <xx:xx:xx:xx:xx:xx>
edit unit 0 /* set logical parameters under this stanza */
set family inet address 192.168.1.100/24 /* multiples addresses per interface can be set */
set family inet address 192.168.1.200/24 preferred /* by default, Junos uses lower adddress when forwaring packtes to locally connected networks */
set family inet address 192.168.1.250/24 primary /* used as source for unicast and multicast packets sourced locally. Also for ip unnumbered. */
rename family inet addresses 192.168.100/24 to addresses 192.168.1.1/24 /* change IP address */
commit and-quit
```
Configure multiple interfaces
```
configure
edit interfaces
set interface-range USERS member-range ge-0/0/1 to ge-0/0/3
edit interface-range USERS /* in case of conflict with individual interface config most specific configuration is used */
/* configure as normal interface */
/* ... */
```

### Ethernet loopback test
Ethernet loopback test allows to check whether the physical interface is working correctly. The device sends test packets out of the interface, which are expected to loop over the plug and back to the interface. If the interface fails to receive any test packets, the hardware of the interface is faulty.

Place an interface in loopback mode

`interfaces fe-fpc/pic/port fastether-options loopback`

To return to the default (disable loopback mode)

`delete interfaces fe-fpc/pic/port fastether-options loopback`

**NOTE:** for Gigabit Ethernet interfaces use `gigether-options`

### Link Aggregation (LAG)
 Set the number of _ae_ interfaces
```
[edit chassis]
aggergated-devices {
  ethernet {
    device-count 1; /* number of physical ae interfaces to be created by the system */
  }
}
```
Edit options of an _ae_ interface
```
[edit interfaces ae0]
aggregated-ether-options {
  lacp {
    passsive;
  }
}
unit 0 {
  family ethernet-switching {
    port-mode trunk;
    vlan {
      members [ orange purple blue];
    }
  }
}
```
Bundle interfaces together
```
[edit interfaces]
ge-0/0/1 {
  gigether-options {
    802.3ad ae0;
  }
}
ge-0/0/2 {
  gigether-options {
    802.3ad ae0;
  }
}
```

## Operational Monitoring and Maintenance

### Chassis monitoring commands
- `show chassis alarms`
- `show chassis hardware` shows all the hardware installed including serial numbers and PICs
- `show chassis hardware clei-models` shows line cards with model numbers
- `show chassis fpc 1` shows number of slots(PICs) in board
- `show chassis fpc pic-status 1` shows PIC information of the FPC 1: type, PIC, status, etc..
- `show chassis ethernet switch` shows the information about the embedded switch used between RE and line cards
- `show chassis craft-interface` craft panel LCD/LEDs statuses
- `show chassis environment` shows temperature and status of boards, power supply, line cards, etc...
- `show chassis routing-engine` shows the status of the Routing Engine (CPU load, temperature, master/slave, model, serial, etc...)
- `show chassis location`
- `show chassis forwarding` Display status of the forwarding process (FWDD). SRX devices
- `show chassis cluster`

### System monitoring commands
- `show system core-dumps`
- `show system alarms`
- `show system boot-messages` displays boot log
- `show system connections` same as _netstat_ in Linux
- `show system licesnse`
- `show system uptime` also shows system clock
- `show system statistics`
- `show system users`
- `show system process extensive` same as _top_ in Linux

### Monitoring interfaces
- `show interfaces terse` similar to the famous `show ip interfaces brief`
- `show interfaces ge-0/0/2 terse` filter by interface. shows all the attached units
- `show interfaces extensive` detailed info about interfaces
- `show interfaces diagnostics optics ge-0/0/0` show info about SFP module
- `monitor interface xe-1/0/0` show interface live statistics
- `monitor interface traffic` show traffic on all interfaces
- `monitor traffic interface ge-0/0/0.0 no-resolve` tcpdump
- `monitor traffic interface ge-0/0/0.0 no-resolve detail` tcpdump
- `monitor traffic interface ge-0/0/0.0 no-resolve write-file MYCAP.pcap` tcpdump
- `monitor traffic interface ge-0/0/0.0 matching "host 192.168.1.1"` tcpdump with custom filter
- `traceroute monitor x.x.x.x` same as _mtr_ in Linux
- `traceroute x.x.x.x` transmits UDP packets and waits for ICPM time-exceeded packets
- `mtrace x.x.x.x` display trace information about an IP multicast path

### Powering on and shutting down Junos devices
- `request system halt` gracefully shut down a Junos device. `both-routing-engine` command is used for routers with redundant Routing Engines. `all-members` is used to shut down EX Series switches when they are configured in a Virtual Chassis configuration.
- `request system power-off` for a standalone chassis (such as MX Series, PTX Series, and T Series routers), the request to power off  the system is applicable only to the Routing Engines.
- `request system reboot`
- `request chassis routing-engine master switch`
- `request chassis fpc slot <x> restart` restart line card in slot x

### Internal storage cleanup
- `show system storage` similar to `df -h` in Linux
- `request system storage cleanup dry-run` shows a list of files that can be deleted
- `request system storage cleanup` delete unnecessary files

### Junos software naming convention
```
junos-install-19.2.R1.8-domestic.tgz
    [package]-[release]-[edition]
```
**NOTE:** Domestic edition has stronger encryption than export

### Junos update from USB
```
start shell
mkdir /tmp/usb
mount -t msdosfs /dev/da1s1 /tmp/usb
exit
request system software add /tmp/usb/junos-srxsme-17.3R2-S4.1.tgz no-copy
```

### Junos update from a network resource
```
file copy ftp://[username:prompt]@[ftp.hostname.net]/[filename] /var/tmp/
request system software add /tmp/usb/junos-srxsme-17.3R2-S4.1.tgz no-copy
```

### Unified ISSU
Unified in-service software upgrade (ISSU) enables you to upgrade between two different Junos releases with minimal disruption on the control plane and with minimal disruption of traffic. It is only available on systems with multiple RE.  Graceful Router Engine Switchover (GRES) and Non Stop Active Routing (NSR) must be enabled. Both RE must run same Junos version before doing an upgrade and there is no possibility to switch on/off any PICs during the upgrade.

To perform the upgrade, run the following command on the master RE:

`request system software in-service-upgrade`

### Set root password
```
configure
set system root-authentication plain-text-password
commit and-quit
```

**NOTE:** Password must be at least 6 characters long and have change of case, digits or punctuation.

### Reset root password
Using console, during boot, hit space bar for command prompt.
```
boot -s /* boot in single user mode */
recovery
set system root-authentication plain-text-password
commit and-quit
```

### Disable password recovery
```
configure
set system ports console insecure /* this will not allow to boot in single user mode */
commit and-quit
```

## Routing
Default Routing Tables:
- `inet.0` IPv4 unicast routes
- `inet.1` Multicast forwarding cache
- `inet.2` Multicast BGP routes for reverse path forwarding (RPF)
- `inet.3` MPLS path info
- `inet.4` Multicast Source Discovery routes (MSDP)
- `inet6.0` IPv6 Unicast routes
- `mpls.0` MPLS next hops

Forwarding table route types (see output of `show route forwarding-table`):
- `dest` Remote address directly reachable through an interface
- `intf` Installed as a result of configuring an interface
- `perm` Installed by the kernel during routing table initialization
- `user` Routes installed by the routing protocol or configuration

Route preference values (lower is better) :
 |      Protocol       | Preference |
 |---------------------|------------|
 | Direct              | 0          |
 | Local               | 0          |
 | Static              | 5          |
 | OSPF Internal       | 10         |
 | RIP                 | 100        |
 | Aggregate (summary) | 130        |
 | OSPF AS external    | 150        |
 | BGP (eBGP and iBGP) | 170        |

### Static, aggregate and generate routes
Static route can have _egress interface name_, _next hop IP address_ or _discard_ as next hop.

A qualified next hop is a configurable option for a static route that allows a static route to have multiple next hops. The qualified next hop can have a different preference, which can allow one next hop to be preferred over another. The qualified next hop is referred to as a _floating static route_ because if the primary next hop for the static route is no longer available, the secondary next hop becomes active.

Whereas a static route is active if the next hop is reachable, which in the case of the special discard and reject next hops means that the static route is always active, for aggregate and generated routes their being active depends on the presence of so-called contributing routes. A contributing route is, simply put, a more specific route than the aggregate or generated route (and sharing the same prefix, of course).

The difference between an aggregate and a generated route is that an aggregate route, even when it's active, can only have `discard` or `reject` next hops, so it will never forward any traffic; a generated route, on the other hand, can have a real next hop, which is taken from the "preferred" contributing route. The next hop of the contributing route can not be reject or discard. 

Configuration example:
```
configure
edit routing-options 
set static route 0.0.0.0/0 next-hop 10.0.0.1 /* with "no-readvertise" this route will not be used by dynamic routing protocols */
set static route 0.0.0.0/0 qualified-next-hop 172.84.23.2 preference 6 /* floating static route (or just route with custom preference value) */
set static route <x.x.x.x/y> reject /* route to null 0 interface and generate icmp unreachable packet (discard is silent) */
set static route <y.y.y.y/z> resolve /* ask Junos for recursive route resolution */
set aggregate route 10.12.0.0/22 /* Creates an aggregate route with REJECT destination */
commit and-quit
```
Verification:
- `show route` displays _inet.0_ and _inet6.0_ routing tables
- `show route instance [<name>]` display all routing instances or filter by name
- `show route forwarding-table` show the forwarding table (contains only ACTIVE routes)
- `show interfaces terse routing-instance <instance-name>` show interfaces of specified routing instance
- `ping <x.x.x.x> routing-instance <instance-name>` perform a _ping_ using specified routing instance
- `traceroute <x.x.x.x> routing-instance <instance-name>` perform a _traceroute_ using specified routing instance.

### ECMP - Equal-cost multipath
By default, ECMP is NOT enabled. If there are multiple routes with same preference and metric values, the router pushes the routing table down to the forwarding table, it randomly selects ONE next-hop. To change that behavior, you can define a `forwarding-table export` policy that controls what happens when the forwarding table gets built from the routing table.

Consider the following configuration:
```
routing-options {
    static {
        route 0.0.0.0/0 {
            next-hop [ 1.1.1.1 1.1.1.2 ];
            metric 10;
        }
    }
}
```
Routing table shows both routes:
```
lab@router> show route 0.0.0.0/0
...
0.0.0.0/0          *[Static/5] 00:01:28, metric 10
                    > to 1.1.1.1 via ge-0/0/0.0
                      to 1.1.1.2 via ge-0/0/0.0
```
But the forwarding table shows that only ONE route is selected:
```
lab@router> show route forwarding-table destination 0.0.0.0/0
...
Destination        Type RtRef Next hop           Type Index NhRef Netif
0.0.0.0/0          user     0 1.1.1.1            ucst   558     3 ge-0/0/0.0
```
Example of ECMP configuration:
```
routing-options {
    static {
        route 0.0.0.0/0 {
            next-hop [ 1.1.1.1 1.1.1.2 ];
            metric 10;
        }
    }
    forwarding-table {
        export LOAD-BALANCE;
    }
}
policy-options {
    policy-statement LOAD-BALANCE {
        then {
            load-balance per-packet; /* this will do per-flow load balancing */
        }
    }
}
```

After configuring ECMP the forwarding table will show `ulst` as next hop type:
```
lab@router> show route forwarding-table destination 0.0.0.0/0                    
...
Destination        Type RtRef Next hop           Type Index NhRef Netif
0.0.0.0/0          user     0                    ulst 262142     2
                              1.1.1.1            ucst   558     3 ge-0/0/0.0
                              1.1.1.2            ucst   540     3 ge-0/0/0.0
```



### URPF - Unicast Reverse Path Forwarding
URPF has 2 modes: strict(default) and loose. Strict checks that the source of return route to the packet is via the same interface. RPF checks are performed on the edge routers, in strict mode this functionality it is enabled on all the interfaces.

`set interface ge-0/0/0 unit 0 family inet rpf-check` enable URPF on interface

```
[edit]
routing-options {
  forwarding-table {
    unicast-reverse-path feasible-paths; /* Enable this otion to consider all feasible paths (assymetric routing). By default: active-paths */
  }
}
ge-0/0/2 {
  unit 0 {
    family inet {
      rpf-check fail-filter rpf-dhcp; /* if the packet is discarded by RPF, it will go to the 'rpf-dhcp' filter. Also the right place to log the discarded packets by RPF (normal filter will not log them /*
      address <x.x.x.x/y>
    }
  }
}
firewall { /* this filter allows DHCP and BOOTP messages to pass the RPF check */
  family inet {
    filter rpf-dhcp {
      from {
        source-address {
          0.0.0.0/32;
        }
        destination-address {
          255.255.255.255/32;
        }
      }
      then accept;
    }
  }
}
```
### Routing instances
A routing instance is a unique collection of routing tables, interfaces and routing protocol parameters.

| Interface        | Description                                                             |
-------------------|-------------------------------------------------------------------------|
| `forwarding`     | Used to implement filter-based forwarding for common layer applications |
| `l2vpn `         | Used in Layer 2 VPN implementations                                     |
| `no-forwarding`  | Used to separate large networks into smaller administrative entities    |
| `virtual-router` | Used for non-VPN-related applications such as system virtualization     |
| `vrf`            | Used in Layer 3 VPN implementations                                     |

```
[edit routing-instances]
CustomInstance {
instance-type virtual-router;
interface fe-1/1/0.0;
interface fe-1/1/1.0;
interface fe-1/1/5.0;
interface ae0.0;
  protocols {
    ospf {
      area 0.0.0.0 {
        interface all;
      }
    }
  }
}
```
### Routing policies
- By default, OSPF import and export policy accepts all the prefixes but rejects everything outside of OSFP.
- In OSPF, policy configuration can be done at protocol level only.
- By default, RIP import policy accepts all and the export policy rejects all (exported prefixes must be specified manually).
- In RIP, import policy can be applied at protocol and neighbor levels.
- By default, BGP import policy accepts all and export policy accept active routes.
- In BGP, policy can apply to neighbor, group or protocol level of config.
- `then` statement is optional.
- Terms are optional in policies.
- If a term does not contain `from` statement, all routes will match.
- Terminating actions: `accept` and `reject`.
- Flow control actions: `next term` and `next policy`.
- If the next `term action` is specified, if no action is specified, or if the route does not match, the evaluation continues.
- If there are no more routing policies, then the accept or reject action specified by the default policy is taken.
- Definition of routing policy is always under [edit policy-options]

### OSPF
```
configure
edit protocols ospf
set area 0 interface ge-0/0/0.0
set area 0 interface ge-0/0/1 /* If the logical interface is not specified, Junos assumes logical unit 0 */
set area 0 interface ge-0/0/2.0 passive /* Do not look for neigbors on this interface but advertice it's IP addresses */
commit and-quit
```
Inject the static default route into OSPF
```
set policy-options policy-statement DEFST term MYTERM from protocol static route-filter 0.0.0.0/0 exact
set policy-options policy-statement DEFST term MYTERM then accept
edit protocols ospf
set export DEFST /* Note that DEFST is applied as an export policy */
commit and-quit
```
Verification:
- `show ospf neighbor [detail|extensive]`
- `show ospf interface`
- `show ospf database`
- `show ospf database advertising-router self` show advertised networks (without network mask)
- `show ospf database advertising-router self detail` show advertised networks, with mask, type, metric, etc...
- `show ospf route` shows where did OSPF learned the routes from and the route types (Extern, Intra, Inter, etc…)
- `show route protocol ospf` filter routing table by OSPF

### RIP
```
[edit protocols]
rip {
  group rip-group {
    export advertise-routes-through-rip;
    neighbor fe-1/2/0.1;
  }
}
[edit policy-options]
policy-statement advertise-routes-through-rip {
  term 1 {
    from protocol [ direct rip ];
    then accept;
  }
}
```
Verification:
- `show route protocol rip` show RIP routes installed into the routing table
- `show route advertising-protocol rip 10.0.0.1` show routes advertised by RIP to neighbors
- `show route receive-protocol rip 10.0.0.2` show routes received by RIP neighbors
- `show rip neighbor` verify that all RIP-enabled Interfaces are available and active
- `show rip statistics` verify that RIP messages are being sent and received on all RIP-enabled interfaces

## Firewall
- In Junos, `lo0` interface is used to filter access to the Routing Engine. Packet Forwarding Engine applies the firewall filters before the traffic reaches the control plane.
- By default, firewall filters are stateless.
- The default firewall filter action is `discard`.
- Actions: `accept`, `discard`, `reject`
- Action modifiers: `count`, `log`, `syslog`, `forwarding-class`, `loss-priority`, etc.. have an implicit accept, the flow control can be changed adding `next term` action modifier.
- `count` action modifier is filter specific. If the filter is applied to another interface, same counter will be used for both interfaces. Use `interface-specific` to overwrite this behavior.
- The log action writes log information to a buffer (line card RAM). There is no option for writing logs to a remote server, or for writing them to disk. Once the available buffer is full, new logs will replace the oldest, so a historical record is not kept. Logs are cleared whenever the device or PFE is restarted.
- Prefix lists can't be used in stateful firewalls!
- `prefix-list-filter` can match `exact`, `longer` or `orlonger`
- `route-filter` is not reusable but it has more match types than a `prefix-list-filter`: `exact` `longer`, `orlonger`, `upto`, `prefix-length-range`.

Configuration example of filtering SSH access to the routing engine:
```
[edit policy-options]
prefix-list trusted {
  172.27.102.0/24;
}
[edit firewall fiter family inet]
filter limit-ssh-access {
  term ssh-accept {
    from {
      source-prefix-list {
        trusted;
      }
      protocol tcp;
      destination-port ssh;
    }
    then accept;
  }
  term ssh-reject {
    from {
      protocol tcp;
      destination-port ssh;
    }
    then {
      discard;
    }
  }
  term else-accept {
    then accept;
  }
}
[edit interfaces]
lo0 {
  unit 0 {
    family inet {
      filter {
        input limit-ssh-access;
      }
      address x.x.x.x/32
    }
  }
}
```
Another configuration example using counters:
```
[edit firewall family inet filter output-ff]
term deny-spoofed {
  from {
    source-address { /* Exclude specific range */
      0.0.0.0/0;
      172.27.102.0/24 except;
    }   
  }
  then {
    log;
    discard;
  }
}
term else-accept {
  then {
    count outbound-accepted;
    accept;
  }
}
```
Verification:
- `show firewall log` show log of filtered packets
- `show firewall counter filter filtername countername` show counter statistics
- `clear firewall filter filtername [counter counter]` clear firewall counter

####  SRX security section
By default,  SRX devices work in _flow mode_ by which security policies are in place and unless otherwise allowed,  packets are dropped (it works as a firewall device). If you want to configure SRX as a router only device, you should change from _flow mode_ to _packet mode_ as below:
```
edit security
set forwarding-options family [inet|inet6] mode packet-based
commit and-quit
request system reboot
```
**NOTE:** For this config to commit properly, security policies must be deactivated or removed.

Verification:
- `show security flow status`

### Policing
- By default, a policer operates in term-specific mode so that, for a given firewall filter, the Junos creates a separate policer instance for every filter term that references the policer. As an option, you can configure a policer to operate in `filter-specific` mode so that a single policer instance is used by all terms (within the same firewall filter) that reference the policer. 
- By default, policers employ token-bucket algorithm.
- To calculate the burst size (best practice), multiply the interface speed (bits per sec) by the desired burst size (in seconds) and divide by 8 to convert it to bytes.
- The actions inside of the policer are ONLY applied  if traffic matches the conditions, otherwise, the actions from the firewall filter will be applied.

Simple policer configuration example:
```
[edit firewall policer P1]
if-exceeding {
  bandwidth-limit 64k; /* in bits (from 30,520 bps to 4.29Gbps) */
  burst-size-limit 4k; /* in bytes (min is 10 times MTU or bandwidth times 3-5ms) */
}
then discard;
```
## CoS - Class of Service
CoS rules must be configured on ALL the network devices consistently, so traffic is treated the same way along the network path.
### Classifiers
 - Default forwarding classes: Best effort (BE), Expedited forwarding (EF), Assured forwarding (AF), Network Control (NC). Best effort is used by default - first in, first out.
- Behavior Aggregate (BA) classifier - set forwarding class and loss priority using header values (DSCP, IP Precedence, MPLS EXP and IEEE 802.1p). Inbound edge devices should SET this fields and the rest of the internal routers should trust this marks.
  - If the _In-the-Box_ model is used, this classifier does not make sense and multifield classifier is used.
  - If the _Across-the-Network_ model is used, the edge device marks the packets with BA and next devices performs CoS based on those values (trust the CoS marks). NO need to use multifield classifier.
- Multifield classifier - allows to set class and loss priority using firewall filters (used to classify by multiple fields of a packet)
- Forwarding Policy options - allows to implement CoS-based forwarding. Balance packets between multiple equal-cost paths or set class and loss priority for prefixes.
- By default, Junos does not modify L3 headers (it possible to do so), but the L2 layer CoS fields must be re-written. IP Precedence and DSCP can not be rewritten at the same time for the same packet because they share common bits.

Default queue to forwarding class associations:
| Queue  | Forwarding class       | Note
|--------|------------------------|----------------------------
|Queue 0 | _Best Effort_          | 95% of traffic goes here
|Queue 1 | _Expedited Forwarding_ | Must be configured manually
|Queue 2 | _Assured Forwarding_   | Must be configured manually
|Queue 3 | _Network control_      | 5% of traffic goes here

BA classifier configuration examples:

Rewrite the CoS markers (at the inbound edge of the network):
```
[edit class-of-service]
interfaces {
  ge-0/0/3 {
    unit 0 {
      rewrite-rules { /* rewrite packet's BA markers */
        inet-precedence default;
      }
    }
  }
}
```
Trust the CoS marks and classify traffic accordingly (applied on the routers inside the network)
```
[edit class-of-service]
interfaces {
  ge-0/0/3 {
    unit 0 {
      classifiers { # read BA makrers and classify traffic accordingly 
        inet-precedence default;
      }
    }
  }
}
```

Multifield classifier configuration example:
```
[edit firewall family inet filter apply-cos-markings]
term admin {
  from {
    source-address {
      x.x.x.x/y
    }
  }
  then {
    forwarding-class expedited-forwarding;
    accept;
  }
}
term all-other-traffic {
  then accept;
}

[edit interfaces ge-0/0/1]
unit 0 {
  family inet {
    filter {
      input apply-cos-markings
    }
    address x.x.x.x/y;
  }
}
```
Example of classifying traffic using policers:
```
firewall {
  policer admin-traffic-policer {
    if-exceeding {
      bandwidth-limit 1m;
      burst-size-limit 3k;
    }
    then forwarding-class best-effort; /* if traffic is exceeding the limits, this class is applied */
  }
  family inet {
    filter apply-cos-markings {
      term admin {
        from {
          source-address {
            x.x.x.x/y
          }
        }
        then {
          policer admin-traffic-policer;
          forwarding-class expedited-forwarding; /* if traffic is NOT exceeding policer limits then this class is applied */
          accept;
        }
      }
      term all-other-traffic {
        then accept;
      }
    }
  }
}
```

### Scheduler and scheduler maps
A scheduler contains parameters about how to service a queue. A scheduler is associated with a queue and forwarding class through a scheduler map. Schedulers work in a WRR (Weighted Round Robbin) format. Custom WRED drop profiles are applied in the schedulers.

Scheduler configuration example:
```
[edit class-of-service schedulers]
sched-BE {
  transmit-rate percent 40; /* adding 'exact' will disallow using any other available bandwidth */
  buffer-size percent 40; /* Same transmit rate and buffer-size percent performs well in many cases */
  priority low;
} /* Custom drop profile not defined, default one works fine in many cases */
```
Scheduler map configuration example
```
[edit class-of-service scheduler-maps]
sched-map-example {
  forwarding-class best-effort scheduler sched-BE;
  forwarding-class expedited-forwarding scheduler sched-EF;
  forwarding-class assured-forwarding scheduler sched-AF;
  forwarding-class network-control scheduler sched-NC;
} /* in this case 4 scheduleras are associated with default forwarding classes (queues) */
```

Example of how to assign scheduler map to interface:
```
[edit class-of-service interface]
fe-* { /* assign a scheduler map to all the interfaces matching this expression */
  scheduler-map sched-map-example;
}
ge-0/0/0 {
  scheduler-map sched-map-example;
}
```
Verification:
- `show class-of-service interface <interface>` display the logical and physical interface associations for the classifier, rewrite rules, and scheduler map objects
- `show class-of-service classifier name <name>` for each class-of-service classifier, display the mapping of code point value to forwarding class and loss priority
- `show interfaces <interface> detail | find "Egress queues"` show egress queue statistics
- `show interface queue ge-0/0/3` show egress queue statistics

### Outbound interfaces queues
Outbound packets are placed in queues and scheduler defines how the interface should process each queue. If the queues are full, the traffic will be dropped until there will be enough space in the queues (tail drop). RED algorithm avoids tail drops by deleting some packets (statistically), helps to avoid global TCP synchronization.

Forwarding classes configuration example. Create and assign forwarding classes to queues.
```
[edit class-of-service]
set forwarding-classes queue 0 general-traffic
set forwarding-classes queue 1 important-traffic
set forwarding-classes queue 2 critical-traffic
```
On most Junos platforms, default forwarding class name are associated with queues 0-3. The previous example overrides that association.
BA classifiers and rewrite rules map traffic to and from queues (regardless of the forwarding class name). All other CoS rules reference forwarding class names, rather than queues.

Verification:
- `show class-of-service forwarding-class` show the association between forwarding classes and the queues

## IPv6

- 128 bit IP address in eight 16-bit hexadecimal blocks separated by colon.
- Does not support NAT.
- Hosts can use stateless address configuration (SLAAC). Small networks with single subnet.
- IPsec support is necessary
- Improved support for options using extension headers. This simplifies the header format.

### IPv4 packet vs IPv6 packet
IPv6 packet structure:
```
+-------------+-------------------+---------------------------------+
| Version (4) | Traffic Class (8) |        Flow Label (20)          |
+-------------+-------------------+---------------------------------+
|       Payload Length (16)       | Next Header (8) | Hop limit (8) |
+---------------------------------+---------------------------------+
|                         Source Address (128)                      |
+-------------------------------------------------------------------+
|                       Destination Address (128)                   |
+-------------------------------------------------------------------+
```
- IPv6 packet has a fixed length of 40bytes while IPv4 has a variable header length
- Both, IPv4 and IPv6 packets, have _Version_, _Source Address_ and _Destination Addresses_ fields.
-  In IPv6 packets,  _Header Length_, _Identification_, _Flags_, _Fragment Offset_ and _Header Checksum_ fields were removed.
- In IPv6 packets, _TOS_ field was renamed to _Traffic Class_. 
- In IPv6 packets, _Time To Live_ field was renamed to _Hop Limit_.

The IPv6 header and extensions headers_ replace the existing IPv4 IP header with options. There are 6 _Extensions Headers_: 
- Hop-by-Hop options - Options to examine by each node along the path
- Routing - list of intermediate nodes that should be visited on the path to dst
- Fragment - signals when a packet has been fragmented by the source. Fragmentation is ALWAYS performed by the source host, using max path MTU discovery mechanism.
- Destination Options - options examined by destination only (can appear twice in a packet)
- Authentication Header - Used with IPsec to verify packet authenticity
- Encrypted Security Payload - Used with IPsec and carries encrypted data

### IPv6 Addresses
Examples of FRC 4291 prefixes:
- `::/128` - Unspecified.
- `::1/128` - Loopback.
- `FF00::/8` - Multicast. Support 16 different types of scope.
- `FE80::/10` - Link local addresses are used for neighbor discovery, autoconfiguration and routing protocol traffic. Used within same routing domain. Automatically generated on all interfaces.
- `FEC0::/10` - Site Local addresses, routable inside a limited area. Can be used as private IPV6 addresses. Have a high probability of global uniqueness.

Recommended prefix lengths (RFC3177)
- Use prefix length `/48` for home networks and enterprises
- Use prefix length `/47` or multiple `/48` for very large subscribers
- Use prefix length `/64` when it is known that one and only one subnet is needed by design.
- Use prefix length `/128` for loopback addresses and end devices

Point to point links
Use prefix length `/127` on inter-router point-to-point links links (RFC6164) but `/64`, `/124` and `/126` have been used for this purpose.

Special addresses:
- `::/16` is reserved for special addressing
- `::1` is a loopback address
- `::` in IPv6 is like 0.0.0.0 in IPv4

#### Global Unicast Address
```
<------Public topology------><-Site-><----Interface identifier---->
+----+-----------------------+-------+----------------------------+
| FP | Global Routing Prefix |  SID  |        Interface ID        |
+-3b-+--------45b------------+--16b--+-----------64b--------------+
```
**FP** - Format Prefix. Binary prefix _001_ (2000::/3) identifies the aggregable global unicast space.

**Global Routing Prefix** - Reserved for hierarchical allocation. Provide higher level of summarization.

**SID** - Subnet Identifier. Reserved for local assignment to a link. Provides subnetting capability within a site.

**Interface ID** - Reserved for interface ID allowing for easy autoconfiguration of a host on a network using IEEE EUI64: MAC Address(48bits) with `0xFFFE` in the middle.

Stateless autoconfiguration:
Host sends RS (Router Solicitation) message to discover the presence of on-link routers. When a router receives RS request from a host, it responds with an RA(Router Advertisement) message which contains prefix list entries that can be used to perform address autoconfiguration. RA messages are also sent periodically by the routers. 

Neighbor discovery tracks the reachability status (and link-layer address) of neighbors on a local link. The device is considered reachable when there is a recent confirmation of a response to NS (Neighbor Solicitation message). Otherwise it is considered unreachable. IPv6 routers can send information of neighbors to downstream nodes. ND is optional on IPv6 devices. 

#### Multicast Address
Multicast addresses are identified by the high order byte FF (1111 1111b)
```
+-----------+-------+-------+----------+
|   8bits   | 4bits | 4bits | 112 bits |
+-----------+-------+-------+----------+
| 1111 1111 | Flags | Scope | Group ID |
+-----------+-------+-------+----------+
```
Type of multicast addresses (RFC4291)
- Solicited-Node Multicast Addresses for NS (Neighbor Solicitation) messages
- All-Nodes Multicast Addresses for RA (Router Advertisement) messages
- All-Routers Multicast Addresses for RS (Router Solicitation) messages

#### Anycast Address (RFC2526)
IPv6 defines a new type of address, known as an "anycast" address, that allows a packet to be routed to one of a number of different nodes all responding to the same address.  The anycast address may be assigned to one or more network interfaces (typically on different nodes), with the network delivering each  packet addressed to  this address to the "nearest" interface based on  the notion of "distance" determined by the routing protocols in use. It also can be used to force routing through a specific ISP.

### IPv6 configuration and verification

Enable IPv6 on an interface:
```
edit interfaces ge-0/0/0 unit 0 
set familiy inet6 /* this enables IPv6 processing on an interface and creates link-local address */
set familiy inet6 address 2001:db8:0:5::/64 eui-64 /* Set an IP address atumatically using EUI64 */
```
Static route configuration:
```
set rib inet6.0 static route 0::/0 next-hop 3001::1
set rib inet6.0 static route 0::/0 qualified-next-hop fe80::1 interface ge-0/0/0 /* If next hop is link-local address, an outgoing interface must be specified because link-local are not globally unique! */
```
Verification:
- `show route table inet6` show all the IPv6 routes
- `show route table inet6.0 protocol static` show IPv6 static routes
- `show ipv6 neighbors` list of neighbors with IPv6 addresses and link-local addresses

#### IPv6 and IPv4 protocols differences:
In IPv6, OSPFv3 (RFC5340) has graceful restart (RFC5781) and authentication/confidentiality (RFC4552) to provide security. The router id is the same as in IPv4 and the protocol configuration is done under [protocols ospf3] stanza.

For protocols such as BGP or IS-IS, everything remains the same, except the neighbor addresses.

### Tunneling IPv6 traffic over IPv4 Networks
Transition from IPv4 to IPv6 by using tunnels to span IPv4 networks until all the intermediate routers have been upgraded to support IPv6.
 
There are few approaches:
- IPv4 compatible addressing
- Configured tunnels
- 6to4
- 6over4

The IPv6 tunneling process consists of the following steps:
- 1 - Encapsulation of the IPv6 packet within an IPv4 header
- 2 - Forwarding of the new IPv4 packet
- 3 - Removal of the IPv4 header
- 4 - Processing or forwarding of the IPv6 packet

Configuration example using GRE tunnel

Topology:
```
+------+ ::2      ::1 +------+              +------+ ::1      ::2 +------+
|USER A|--------------|  R1  |--------------|  R2  |--------------|USER B|
+------+    Site A    +------+   Internet   +------+    Site B    +------+
      fc00:0:0:2000::/64                          fc00:0:0:2001::/64
```

R1 Tunnel Interface configuration:
```
[edit interfaces gr-0/0/0]
unit 0 {
  tunnel {
    source 192.168.1.1;
    destination 192.168.2.1;
  }
  family inet6 {
    address fc00:0:0:1000::1/126;
  }
}
```

R2 Tunnel Interface configuration:
```
[edit interfaces gr-0/0/0]
unit 0 {
  tunnel {
    source 192.168.2.1;
    destination 192.168.1.1;
  }
  family inet6 {
    address fc00:0:0:1000::2/126;
  }
}
```

R1 Route configuration:
```
[edit routing options]
rib inet6.0 {
  static {
    route fc00:0:0:2001::/64 next-hop gr-0/0/0.0;
  }
}
static {
  route 192.168.2.1/32 next hop 172.18.1.1;
}
```

R2 Route configuration:
```
[edit routing options]
rib inet6.0 {
  static {
    route fc00:0:0:2000::/64 next-hop gr-0/0/0.0;
  }
}
static {
  route 192.168.1.1/32 next hop 172.18.2.1;
}
```
Verification:
- `show interfaces gr-0/0/0 terse` verify that tunnel interface is up on both routers
- `show route 192.168.2.1` verify that the required routes are installed on both routers
- `show route table inet6.0 fc00:0:0:2001::/64` verify that the required routes are installed on both routers
- `ping fc00:0:0:2001::2 source fc00:0:0:2001:1 rapid count 25` verify the connectivity
- `show interfaces gr-0/0/0.0 detail | find "traffic statistics"` verify tunnel usage statistics

**NOTE**: GRE tunnels are stateless.

## JTAC
**YOU MUST HAVE A VALID SERVICE CONTRACT!**

- `show chassis hardware` get chassis serial number to provide it to a JTAC support
- `request support information` provides additional information for JTAC
- Every case has a priority (1 - Critical, 2 - High, 3 - Medium, 4 - Low)
- It is possible to escalate the case manually or calling JTAC
- Juniper has KB (Knowledge Base) - repository of useful information (documents, troubleshooting, technology notes, etc..)
- KB also has [VPN Configuration Tool](https://support.juniper.net/support/tools/vpnconfig/), [SRX HA Configuration Tool](https://support.juniper.net/support/tools/srxha/) and IOS to Junos Translator (The IOS-to-Junos conversion tool is no longer available)
- PR - Problem Report
- If you need to submit a file larger than 10M (i.e.: core file), then, you need to transfer it to `sftp.juniper.net` to _/pub/incoming_ folder with case ID.

Example of uploading a file to Juniper's FTP server:
```
sftp anonymous@sftp.juniper.net
sftp> lcd /var/tmp
sftp> cd /pub/incoming
sftp> mkdir xxx-xxx-xxx
sftp> cd xxx-xxx-xxx
sftp> mput large-file.tgz
sftp> bye
```

## Security concepts
Juniper's viewpoint on security can be grouped into three different categories:
- Operation efficiency - centralized management and control
- Security efficacy - advanced security services, thread intelligence and fine-grained policy controls
- Business agility - scalability of deployment

5 Focus points for implementing secure network:
- Performance - How the network will perform across wide range of user cases
- Efficacy - How to measure the ability of identify and mitigate threats (protection quality)
- Scalability - How the network will scale as traffic increases
- Automation - How programmable the solution will be
- Centralized control - How to control the solution from a single location

Juniper SRX can track apps and block the risky ones (allowing user-tailored policies). It can prioritize important apps and define packet forwarding rules. Also offers SSL packet inspection and IPS services.

### UTM - Unified Thread Management
- Sophos antivirus - in-the-cloud antivirus with small footprint
- Antispam filtering - e-mail filter (separate licensing)
- Web filtering 
  - Enhanced Web filtering—The enhanced Web filtering solution intercepts the HTTP and the HTTPS requests and sends the HTTP URL or the HTTPS source IP to the Websense ThreatSeeker Cloud (TSC). The TSC categorizes the URL into one of the categories that are predefined and also provides site reputation information. The TSC further returns the URL category and the site reputation information to the device. The device determines if it can permit or block the request based on the information provided by the TSC.
  - Redirect Web filtering—The redirect Web filtering solution intercepts HTTP and HTTPS requests and sends them to an external URL filtering server, provided by Websense, to determine whether to block the requests. Redirect Web filtering does not require a license.
  - Local Web filtering—The local Web filtering solution intercepts every HTTP request and the HTTPS request in a TCP connection. In this case, the decision making is done on the device after it looks up a URL to determine if it is in the allow list or blocklist based on its user-defined category. Local Web filtering does not require a license or a remote category server.
  - Content filter - blocks or permits certain types of traffic based on MIME, extension, protocol and object type. Dos not require separate license.
- Sky ATP - Advanced Thread Prevention. Key component of Juniper Connected Security. Advanced, cloud based malware protection using machine learning.
