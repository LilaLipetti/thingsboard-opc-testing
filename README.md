# thingsboard-opc-testing
Testing thingsboard opc gateway + KEPServerEX 


# TEST 1
- configured the thingsboard and thingsboard opc gateway as described in https://thingsboard.io/docs/iot-gateway/getting-started/#step-9-connect-to-external-opc-ua-server
- debug log in tb-gateway_1.log
- based on code there should be some method calls to  
     public void scanForDevices() 
     => not so easy to detect the logs from this method
     and to   
     private void scanForDevices(OpcUaNode node)
     => no debug prints from  private void scanForDevices(OpcUaNode node)

# TEST 2
- changed the thingsboard opc gateway configuration to
```
{
  "deviceNodePattern": "Channel1",
  "deviceNamePattern": "Channel1",
  "attributes": [
    {"key":"Tag1", "type": "string", "value": "${Tag1}"}
  ],
  "timeseries": [
    {"key":"Tag2", "type": "long", "value": "${Tag2}"}
  ]
}
```
- debug log in tb-gateway_2.log, at least now it tries to scan the tree for tags i.e there are debug output from  
  private void scanForDevices(OpcUaNode node)
 ``` 
  2017-11-06 14:49:55,568 [main] DEBUG o.t.g.e.opc.OpcUaServerMonitor - Scanning device node: OpcUaNode(nodeId=NodeId{ns=2, id=Channel1.Device1}, name=Device1, fqn=Objects.Channel1.Device1)
2017-11-06 14:49:55,571 [main] DEBUG o.t.g.e.opc.OpcUaServerMonitor - Scanning node hierarchy for tags: [Tag1, Tag2]
2017-11-06 14:49:55,591 [main] ERROR o.t.g.e.opc.OpcUaServerMonitor - Failed to scan device: Device1
java.lang.StringIndexOutOfBoundsException: String index out of range: -1
	at java.lang.String.substring(String.java:1931)
	at org.thingsboard.gateway.extensions.opc.OpcUaServerMonitor.lookupTags(OpcUaServerMonitor.java:319)
	at org.thingsboard.gateway.extensions.opc.OpcUaServerMonitor.scanDevice(OpcUaServerMonitor.java:191)
	at org.thingsboard.gateway.extensions.opc.OpcUaServerMonitor.lambda$scanForDevices$8(OpcUaServerMonitor.java:159)
	at java.util.ArrayList.forEach(ArrayList.java:1255)
```
The OpcUaNode(nodeId=NodeId{ns=2, id=Channel1.Device1}, name=Device1, fqn=Objects.Channel1.Device1) seems to be valid
