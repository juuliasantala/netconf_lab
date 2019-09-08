# Netconf_lab
DevNet Partner University, 10th - 11th September 2019

## 1. Cloning the YANG models:
git clone https://github.com/YangModels/yang

## 2. Using pyang to check review YANG models:
pyang -f tree --ignore-errors --tree-path *YANG-model*


------------------------------


## 3. NETCONF to the router
ssh *username*@*router-IP* -p 830

## 4. NETCONF Hello message
```
<?xml version="1.0" encoding="UTF-8"?>
    <hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
        <capabilities>
            <capability>
                urn:ietf:params:netconf:base:1.0
            </capability>
        </capabilities>
    </hello>]]>]]>
```

## 5. Vendor definition: Check hostname
```
<?xml version="1.0" encoding="UTF-8"?>
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <get>
    <filter>
      <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
		<hostname/>
      </native>
    </filter>
  </get>
</rpc>]]>]]>
```

## 6. Industry standard: Get interfaces
```
<?xml version="1.0" encoding="UTF-8"?>
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<get>
    <filter>
      <interfaces xmlns="urn:ietf:params:xml:ns:yang:ietf-interfaces">
		<interface></interface>
</interfaces> 
</filter>
  </get>
</rpc>]]>]]>
```

## 7. Let's change the hostname!
```
<?xml version="1.0" encoding="UTF-8"?>
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"> 
   <edit-config>
      <target>
         <running/>
      </target>
    <config>
            <native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
                <hostname>Netconf_switch</hostname>
            </native>
    </config>
   </edit-config>
</rpc>]]>]]>
```
## 8. Remember to close the session
```
<?xml version="1.0" encoding="UTF-8"?> <rpc message-id="1239123" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"> <close-session /> </rpc> ]]>]]>
```


------------------------------


## 9. Start Python code by defining the libraries
```python
from ncclient import manager
import xmltodict
import json
```

## 10. Create the NETCONF connection over SSH
````python
try:
    m = manager.connect(
        host={{host_IP}},
        port={{netconf_port}},
        username={{username}},
        password={{password}},
        hostkey_verify=False
        )
except:
    print("Unable to connect to the router.")
````
