# Test Plan

| Test | Expected result |
|---|---|
| VLAN verification | Required VLANs present |
| Trunk verification | Required trunks up |
| Router interfaces | Subinterfaces up/up |
| DHCP | Clients receive correct subnet |
| Staff → server | Allowed |
| Staff → Internet | Allowed |
| DNS | Internal hostname resolves |
| NAT | Translation entries appear |
| Guest → internal server | Blocked |
| Guest → management | Blocked |
| Guest → Internet | Allowed |
| WPA2 Staff | WPA2-PSK/AES |
| WPA2 Guest | WPA2-PSK/AES |
| SSH | Administrative login succeeds |
| Routing | Connected/default routes present |

Only mark tests PASS after testing the student's own Packet Tracer file and capturing evidence.
