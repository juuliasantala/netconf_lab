<h1>Netconf_lab</h1>
<p>DevNet Partner University, 10th - 11th September 2019</p>

<h2>1. Cloning the YANG models:</h2>
git clone https://github.com/YangModels/yang

<h2>2. Using pyang to check review YANG models:</h2>
pyang -f tree --ignore-errors --tree-path <i>YANG-model</i>

<hr>

<h2>3. NETCONF to the router</h2>
ssh <i>username</i>@<i>router-IP</i> -p 830

<h2>4. NETCONF Hello message</h2>
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
<h2>5. Vendor definition: Check hostname</h2>
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
<h2>6. Industry standard: Get interfaces</h2>
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
<h2>7. Let's change the hostname!</h2>
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
<h2>8. Remember to close the session</h2>
```
<?xml version="1.0" encoding="UTF-8"?> <rpc message-id="1239123" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0"> <close-session /> </rpc> ]]>]]>

