# p2p-port-mapping
p2p port forwarding/mapping across NAT/firewalls, Access your server anywhere.
<br />
<a href="https://github.com/yuanzhanghu/p2p-port-mapping/blob/master/README_Chinese.md"><strong>中文说明</strong></a>
## Features
```
- p2p port forwarding/mapping, no data relay server is needed.
- No administrator / root privilege is required.
- NAT traversal without router configuration.
- support multiple clients and multiple connections for each tunnel.
- using node-datachannel to establish data tunnel, scp rate can reach upto 400Mbps for the tunnel.
```
## Installation
```
1. install nodejs
2. git clone https://github.com/yuanzhanghu/p2p-port-mapping.git
3. cd p2p-port-mapping
4. npm install
```
## Example 1 (verified on linux)
```
Assume that we want to do ssh from computer B to computer A across firewalls, we can do port mapping like:
1. on computer A (which want to share port), mapping port to serverKey:
 node main_server.js --port 22

get printed log:
 2023-11-08 19:37:10 mappingServer info: server_registered, local port:22 ====> serverKey:KJASD2DW2 

please write down the serverKey.

2. on computer B, mapping serverKey in to a localhost port:
// above serverKey is used on computer B
  node main_client.js --port 8082 --key <serverKey-displayed-in-step1>

get printed log:
2023-11-08 19:37:20 mappingClient info: tunnel established. serverKey:KJASD2DW2 ====> local port:8082


3. now we can do this on B:
ssh user@localhost -p 8082

above command will ssh to A actually.
```

## Example 2 (verified on linux and windows)
1. Assume that we want to do remote desktop access from computer B to computer A across firewalls.
On computer A (which want to share desktop), enable remote desktop sharing, mapping port to serverKey:
```
 node main_server.js --port 3389

get printed log:
 2023-11-16 17:37:10 mappingServer info: server_registered, local port:3389 ====> serverKey:KJASD2DW2
```
write down the serverKey, which will be used in step 2.
<br>
2. on computer B, do this:
```
 node main_server.js --key KJASD2DW2 --port 9389 

get printed log:
2023-11-16 17:37:41 startClient info: tunnel established. serverKey:KJASD2DW2 ====> local port:9389
```
3. Then  on computer B: we can do remote desktop access to localhost:9389, which is actually access to A.

## Signal Server For WebRTC
Current signal server is using socket.io running on ai1to1.com, you can run your signal server as well. Source code is located in signal_server/, just run:
```
python3 api_server.py
```

## STUN / TURN Server For WebRTC
Currently using:
```
[
  'stun:stun.l.google.com:19302',
  'turn:free:free@freeturn.net:3478',
]
```
You can change it in webRTC.js

## Contact
email: huyuanzhang@gmail.com
