# Synology SNMP + Zabbix setup [DSM 7]

## Zabbix running inside docker VM setup
Minimum RAM size for VM: 768MB

https://www.schaupper.at/synology-zabbix-docker/

Use docker-compose.yaml from this repo

## Synology setup
```
control panel -> terminal & snmp -> SNMP -> Enable SNMP service
```
```
  SNMPv3 service:
  
    username: <your_username>
    protocol: SHA
    password: <your_password>
        
  SNMP Device Information:
    device name: <your_device_name>
```
## download template
https://github.com/zabbix/community-templates/blob/main/Storage_Devices/Synology/template_synology_diskstation_snmpv3/6.0/template_synology_diskstation_snmpv3.yaml

## Zabbix setup
### import template
```
zabbix -> configuration -> templates -> import -> choose "template_synology_diskstation_snmpv3.yaml" -> import -> import
```
### create host
```
zabbix -> configuration -> hosts -> create host:

  [HOST]
    Hostname: <your_host_name>
    Templates: Select -> Select -> Templates -> Synology DiskStation SNMPv3
    Groups: Linux Servers
    Interfaces: SNMP
      IP Address: <your_synology_ip>
      SNMP Version: SNMPv3
      Security name: <username_from_DSM>
      Security level: authNoPriv
      Authentication protocol: SHA1
      Authentication passphrase: <password_from_DSM>
      
  [Macros]
    -> Add:
        Macro: {$SNMP_AUTHPASS}
        Value: <password_from_DSM>
    -> Add:
        Macro: {$SNMP_USERNAME}
        Value: <username_from_DSM>    
        
  [Inventory]
    -> Automatic

  -> Add
```
