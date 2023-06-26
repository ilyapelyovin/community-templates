# Mesos by HTTP

## Description

This template works out of the box as soon as Mesos Observability Metrics is available inside your cluster; it does not require any Zabbix agent installation or configuration. It allows external monitoring of the Mesos cluster through the metrics HTTP endpoint.

## Overview

 ### Description


zabbix-mesos queries the Observability metrics endpoint, as it's described in [Observability Metrics](https://mesos.apache.org/documentation/latest/monitoring/).


### Installation


1. Import global Zabbix Template (zabbix-mesos-via-http.xml) into your Zabbix server.
2. Create or import hosts identifying your Mesos cluster masters.
3. Assign 'Template App Mesos Master via HTTP' template to Mesos masters


### Templates


The global export (zabbix-mesos-master.xml) contains following templates:




| Templates | Description |
| --- | --- |
| Template App Mesos Master via HTTP | Template applied to the Mesos masters. |
| Template App Mesos Agent via HTTP | Template applied to the Mesos masters. |


### Licenses




| Template | License |
| --- | --- |
| Template App Mesos Master via HTTP | *GNU General Public License v3.0* [Copyright (C) 2023 Erasys GmbH](https://www.erasys.de/) |
| Template App Mesos Agent via HTTP | *GNU General Public License v3.0* [Copyright (C) 2023 Erasys GmbH](https://www.erasys.de/) |




---


 
# Template App Mesos Master via HTTP


## Author

Ilya Pelevin

## Macros used

|Name|Description|Default|Type|
|----|-----------|-------|----|
|{$MESOS_CPUS_PERCENT_CRIT}|<p>Maximal tolerated load on the CPUs</p>|`90`|Text macro|
|{$MESOS_CPUS_PERCENT_WARN}|<p>Warning threshold for load on the CPUs</p>|`80`|Text macro|
|{$MESOS_MEMORY_PERCENT_CRIT}|<p>Maximal tolerated load on the memory</p>|`90`|Text macro|
|{$MESOS_MEMORY_PERCENT_WARN}|<p>Warning threshold for load on the memory</p>|`80`|Text macro|
|{$MESOS_MIN_SLAVES_CRIT}|<p>Minimal tolerated number of Mesos agents</p>|`65`|Text macro|
|{$MESOS_MIN_SLAVES_WARN}|<p>Warning threshold for number of Mesos agents</p>|`70`|Text macro|


## Template links

There are no template links in this template.

## Discovery rules

There are no discovery rules in this template.


## Items collected

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|Load average (1m avg)|<p>-</p>|`Dependent item`|system.cpu.load.avg1[node_exporter]<p>Update: 0</p>|
|CPU guest time|<p>Guest time (time spent running a virtual CPU for a guest operating system)</p>|`Dependent item`|system.cpu.guest[node_exporter]<p>Update: 0</p>|
|CPU idle time|<p>The time the CPU has spent doing nothing.</p>|`Dependent item`|system.cpu.idle[node_exporter]<p>Update: 0</p>|
|Operating system architecture|<p>Operating system architecture of the host.</p>|`Dependent item`|system.sw.arch[node_exporter]<p>Update: 0</p>|
|System boot time|<p>-</p>|`Dependent item`|system.boottime[node_exporter]<p>Update: 0</p>|
|Interrupts per second|<p>-</p>|`Dependent item`|system.cpu.intr[node_exporter]<p>Update: 0</p>|

## Triggers

|Name|Description|Expression|Priority|
|----|-----------|----------|--------|
|Interface {#IFNAME}({#IFALIAS}): Ethernet has changed to lower speed than it was before|<p>This Ethernet connection has transitioned down from its known maximum speed. This might be a sign of autonegotiation issues. Ack to close.</p>|<p>**Expression**: change(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])<0 and last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])>0 and ( last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=6 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=7 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=11 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=62 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=69 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=117 ) and (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])<>2)</p><p>**Recovery expression**: (change(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])>0 and last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"],#2)>0) or (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])=2)</p>|information|
|Interface {#IFNAME}({#IFALIAS}): Ethernet has changed to lower speed than it was before|<p>This Ethernet connection has transitioned down from its known maximum speed. This might be a sign of autonegotiation issues. Ack to close.</p>|<p>**Expression**: change(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])<0 and last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])>0 and (last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=6 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=1) and (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])<>2)</p><p>**Recovery expression**: (change(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])>0 and last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"],#2)>0) or (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])=2)</p>|information|
|Interface {#IFNAME}({#IFALIAS}): High bandwidth usage ( > {$IF.UTIL.MAX:"{#IFNAME}"}% )|<p>The network interface utilization is close to its estimated maximum bandwidth.</p>|<p>**Expression**: (avg(/Kube Node by Prom API/net.if.in[node_exporter,"{#IFNAME}"],15m)>(90/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"]) or avg(/Kube Node by Prom API/net.if.out[node_exporter,"{#IFNAME}"],15m)>(90/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])) and last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])>0</p><p>**Recovery expression**: avg(/Kube Node by Prom API/net.if.in[node_exporter,"{#IFNAME}"],15m)<((90-3)/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"]) and avg(/Kube Node by Prom API/net.if.out[node_exporter,"{#IFNAME}"],15m)<((90-3)/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])</p>|warning|
|Interface {#IFNAME}({#IFALIAS}): High error rate ( > {$IF.ERRORS.WARN:"{#IFNAME}"} for 5m)|<p>Recovers when below 80% of {$IF.ERRORS.WARN:"{#IFNAME}"} threshold</p>|<p>**Expression**: min(/Kube Node by Prom API/net.if.in.errors[node_exporter,"{#IFNAME}"],5m)>2 or min(/Kube Node by Prom API/net.if.out.errors[node_exporter"{#IFNAME}"],5m)>2</p><p>**Recovery expression**: max(/Kube Node by Prom API/net.if.in.errors[node_exporter,"{#IFNAME}"],5m)<2*0.8 and max(/Kube Node by Prom API/net.if.out.errors[node_exporter"{#IFNAME}"],5m)<2*0.8</p>|warning|
|Interface {#IFNAME}({#IFALIAS}): 


# Template App Mesos Agent via HTTP


## Author

Ilya Pelevin

## Macros used

|Name|Description|Default|Type|
|----|-----------|-------|----|
|{$MESOS_AGENT_CPUS_CRIT}|<p>Maximal tolerated load on the CPUs</p>|`90`|Text macro|
|{$MESOS_AGENT_CPUS_WARN}|<p>Warning threshold for load on the CPUs</p>|`80`|Text macro|
|{$MESOS_AGENT_CPUS_MIN}|<p>WMinimum expected agent CPU allocation</p>|`50`|Text macro|
|{$MESOS_AGENT_CPUS_WARN}|<p>Maximal tolerated load on the memory</p>|`90`|Text macro|
|{$MESOS_AGENT_MEMORY_CRIT}|<p>Maximum tolerated agent memory allocation</p>|`90`|Text macro|
|{$MESOS_AGENT_MEMORY_WARN}|<p>Warning threshold for mesos agent memory allocation</p>|`80`|Text macro|
|{$MESOS_AGENT_DISK_CRIT}|<p>Maximum tolerated agent disk allocation</p>|`90`|Text macro|
|{$MESOS_AGENT_DISK_WARN}|<p>Warning threshold for mesos agent disk allocation</p>|`80`|Text macro|

## Template links

There are no template links in this template.

## Discovery rules

There are no discovery rules in this template.


## Items collected

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|Load average (1m avg)|<p>-</p>|`Dependent item`|system.cpu.load.avg1[node_exporter]<p>Update: 0</p>|
|CPU guest time|<p>Guest time (time spent running a virtual CPU for a guest operating system)</p>|`Dependent item`|system.cpu.guest[node_exporter]<p>Update: 0</p>|
|CPU idle time|<p>The time the CPU has spent doing nothing.</p>|`Dependent item`|system.cpu.idle[node_exporter]<p>Update: 0</p>|
|Operating system architecture|<p>Operating system architecture of the host.</p>|`Dependent item`|system.sw.arch[node_exporter]<p>Update: 0</p>|
|System boot time|<p>-</p>|`Dependent item`|system.boottime[node_exporter]<p>Update: 0</p>|
|Interrupts per second|<p>-</p>|`Dependent item`|system.cpu.intr[node_exporter]<p>Update: 0</p>|

## Triggers

|Name|Description|Expression|Priority|
|----|-----------|----------|--------|
|Interface {#IFNAME}({#IFALIAS}): Ethernet has changed to lower speed than it was before|<p>This Ethernet connection has transitioned down from its known maximum speed. This might be a sign of autonegotiation issues. Ack to close.</p>|<p>**Expression**: change(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])<0 and last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])>0 and ( last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=6 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=7 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=11 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=62 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=69 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=117 ) and (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])<>2)</p><p>**Recovery expression**: (change(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])>0 and last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"],#2)>0) or (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])=2)</p>|information|
|Interface {#IFNAME}({#IFALIAS}): Ethernet has changed to lower speed than it was before|<p>This Ethernet connection has transitioned down from its known maximum speed. This might be a sign of autonegotiation issues. Ack to close.</p>|<p>**Expression**: change(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])<0 and last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])>0 and (last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=6 or last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])=1) and (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])<>2)</p><p>**Recovery expression**: (change(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"])>0 and last(/Kube Node by Prom API/net.if.type[node_exporter,"{#IFNAME}"],#2)>0) or (last(/Kube Node by Prom API/net.if.status[node_exporter,"{#IFNAME}"])=2)</p>|information|
|Interface {#IFNAME}({#IFALIAS}): High bandwidth usage ( > {$IF.UTIL.MAX:"{#IFNAME}"}% )|<p>The network interface utilization is close to its estimated maximum bandwidth.</p>|<p>**Expression**: (avg(/Kube Node by Prom API/net.if.in[node_exporter,"{#IFNAME}"],15m)>(90/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"]) or avg(/Kube Node by Prom API/net.if.out[node_exporter,"{#IFNAME}"],15m)>(90/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])) and last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])>0</p><p>**Recovery expression**: avg(/Kube Node by Prom API/net.if.in[node_exporter,"{#IFNAME}"],15m)<((90-3)/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"]) and avg(/Kube Node by Prom API/net.if.out[node_exporter,"{#IFNAME}"],15m)<((90-3)/100)*last(/Kube Node by Prom API/net.if.speed[node_exporter,"{#IFNAME}"])</p>|warning|
|Interface {#IFNAME}({#IFALIAS}): High error rate ( > {$IF.ERRORS.WARN:"{#IFNAME}"} for 5m)|<p>Recovers when below 80% of {$IF.ERRORS.WARN:"{#IFNAME}"} threshold</p>|<p>**Expression**: min(/Kube Node by Prom API/net.if.in.errors[node_exporter,"{#IFNAME}"],5m)>2 or min(/Kube Node by Prom API/net.if.out.errors[node_exporter"{#IFNAME}"],5m)>2</p><p>**Recovery expression**: max(/Kube Node by Prom API/net.if.in.errors[node_exporter,"{#IFNAME}"],5m)<2*0.8 and max(/Kube Node by Prom API/net.if.out.errors[node_exporter"{#IFNAME}"],5m)<2*0.8</p>|warning|
|Interface {#IFNAME}({#IFALIAS}): 
