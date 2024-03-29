# Netconf_lab
DevNet Partner University, 10th - 11th September 2019

## Before we start...
You need
- Python 3 (Don't have Python 3? https://realpython.com/installing-python/)
- The following Python libraries:
***ncclient, xmltodict, json, pyang***
(Windows and pip3 not installed? https://phoenixnap.com/kb/install-pip-windows)

Do you have an editor to work on Python? If not, for example Sublime Text is nice and simple editor: https://www.sublimetext.com/

## 1. Cloning the YANG models:
git clone https://github.com/YangModels/yang

## 2. Using pyang to check review YANG models:
pyang -f tree *YANG-model*


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

or

```
<?xml version="1.0" encoding="UTF-8"?>
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<get-config>
<source>
<running/>
</source>

<filter>
<native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
<hostname/>
</native>
</filter>
</get-config>
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
<?xml version="1.0" encoding="UTF-8"?> 
<rpc message-id="1239123" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"> 
<close-session /> 
</rpc> ]]>]]>
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

Also add a line in the end of the document to close the connection:
```python
m.close_session()
```

## 11. Create your first filter
```python
netconf_filter = """
<filter>
<native xmlns="http://cisco.com/ns/yang/Cisco-IOS-XE-native">
<hostname/>
</native>
</filter>
"""
```

## 12. Let's use the filter!
```python
netconf_reply = m.get_config(source = 'running', filter = netconf_filter).data_xml
```

## 13. Making dictionary out of the response
```python
netconf_reply_dict = json.loads(json.dumps(xmltodict.parse(netconf_reply)))
```

## 14. Review the output in pretty JSON
```python
print(json.dumps(netconf_reply_dict, indent=4, sort_keys=True))
```

## 15. Access the hostname in the response
```python
hostname = netconf_reply_dict["data"]["native"]["hostname"]
print(hostname)
```

### 16. Now that we know how you can use NETCONF in your Python code, let's create a code that tells us if an interface on the device is down.
