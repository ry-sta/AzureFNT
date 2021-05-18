# Deployment configuration

After deployment, the following deployment script should be added to the FortiGate. Ensure the script and the IP Addresses match the deployment requirements.

## FortiGate A

```
config system sdn-connector
  edit AzureSDN
    set type azure
  next
config system interface
edit "port1"
        set vdom "root"
        set ip 10.40.1.5 255.255.255.0
        set allowaccess ping https ssh fgfm
        set type physical
        set snmp-index 1
        set secondary-IP enable
        config secondaryip
            edit 1
                set ip 10.40.1.6 255.255.255.0
                set allowaccess ping
            next
        end
    next
config firewall vip
    edit "Win10 VIP"
        set uuid e2098758-b703-51eb-2488-f82fcf151dd0
        set extip 10.40.1.6
        set mappedip "10.50.1.4"
        set extintf "port1"
    next
config firewall policy
    edit 1
        set name "Win 10Access"
        set srcintf "port1"
        set dstintf "port2"
        set srcaddr "all"
        set dstaddr "Win10 VIP"
        set action accept
        set schedule "always"
        set service "RDP"
    next
    edit 2
        set name "All out"
        set srcintf "port2"
        set dstintf "port1"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
config router static
    edit 1
        set gateway 10.40.1.1
        set device "port1"
    next
    edit 2
        set dst 10.40.0.0 255.255.0.0
        set gateway 10.40.2.1
        set device "port2"
    next
    edit 3
        set dst 10.50.0.0 255.255.0.0
        set gateway 10.40.2.1
        set device "port2"
    next
end
</pre>
```
