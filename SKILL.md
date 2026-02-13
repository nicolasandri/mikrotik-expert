# MikroTik Expert

Expert RouterOS administrator specializing in ISP/WISP operations, PPPoE management, hotspot systems, and advanced network troubleshooting for customer premise equipment.

## When to Use This Skill

- Configuring MikroTik routers for ISP/WISP deployments
- Troubleshooting customer connectivity issues (PPPoE, DHCP, Hotspot)
- Diagnosing bandwidth, latency, and packet loss problems
- Managing VLANs, QoS, and traffic shaping
- Scripting automation for monitoring and maintenance
- Resolving firewall/NAT issues

## Core Workflows

### Workflow 1: PPPoE Connection Troubleshooting

**Step 1: Check PPPoE Client Status**
```
/interface pppoe-client print
/interface pppoe-client monitor [find name=pppoe-out1]
```
- Verify status: "connected" or "disconnected"
- Check for error messages

**Step 2: Verify Physical Layer**
```
/interface ethernet print
/interface ethernet monitor [find name=ether1]
```
- Confirm cable connected (status: link-ok)
- Check for RX/TX errors

**Step 3: Test Authentication**
```
/log print where topics~"ppp"
```
- Look for authentication failures
- Verify username/password
- Check RADIUS server connectivity (if used)

**Step 4: Common Fixes**
- Wrong credentials: Update `/ppp secret` or inform customer
- VLAN mismatch: Check `/interface vlan` configuration
- MTU issues: Set `mtu=1480` and `mru=1480` on PPPoE client
- Service name mismatch: Ensure service-name matches ISP config

---

### Workflow 2: Bandwidth/Latency Diagnostics

**Step 1: Test Connectivity**
```
/ping 8.8.8.8 count=100
/tool bandwidth-test 8.8.8.8 direction=both duration=10s
```

**Step 2: Check for Packet Loss**
```
/interface print stats
/interface ethernet print stats
```
- Look for RX/TX drops
- Check for CRC errors

**Step 3: Analyze Queue Tree**
```
/queue tree print
/queue simple print
```
- Verify bandwidth limits not exceeded
- Check for queue drops

**Step 4: CPU and Memory Check**
```
/system resource print
/system resource cpu print
```
- High CPU can cause latency spikes
- Low memory affects performance

---

### Workflow 3: Hotspot User Issues

**Step 1: Check Hotspot Status**
```
/ip hotspot print
/ip hotspot host print
/ip hotspot active print
```

**Step 2: Verify User Authentication**
```
/ip hotspot user print
/ip hotspot cookie print
```
- Check if user exists
- Verify profile assignment

**Step 3: Common Fixes**
- User locked: `/ip hotspot user reset-counters [find name=username]`
- Session expired: Check profile limits
- MAC binding issues: Remove old bindings
- DNS redirect not working: Check `/ip hotspot walled-garden`

---

### Workflow 4: Firewall/NAT Troubleshooting

**Step 1: Check NAT Rules**
```
/ip firewall nat print
/ip firewall nat print stats
```
- Verify masquerade rule exists
- Check for conflicting rules

**Step 2: Check Filter Rules**
```
/ip firewall filter print
/ip firewall filter print stats
```
- Look for DROP rules blocking traffic
- Verify rule order (top to bottom)

**Step 3: Connection Tracking**
```
/ip firewall connection print count-only
/ip firewall connection print where src-address~"customer-ip"
```
- Check connection count (max 2M on most devices)
- Look for connection flooding

---

### Workflow 5: VLAN Configuration Check

**Step 1: Verify VLAN Setup**
```
/interface vlan print
/interface bridge port print
```

**Step 2: Check VLAN Tagging**
```
/interface bridge vlan print
```
- Ensure tagged/untagged ports correct
- Verify VLAN IDs match ISP requirements

---

## Quick Reference Commands

### System Information
```
/system identity print
/system resource print
/system routerboard print
/system license print
```

### Interface Status
```
/interface print
/interface ethernet print detail
/interface pppoe-client print
```

### IP Configuration
```
/ip address print
/ip route print
/ip dns print
/ip dhcp-server print
```

### User Management
```
/user print
/ppp secret print
/ip hotspot user print
```

### Logs and Monitoring
```
/log print
/log print follow
/tool torch
/tool sniffer
```

## Common Issues & Solutions

### Issue: PPPoE Keeps Disconnecting
**Causes:**
- Keepalive timeout too short
- MTU/MRU mismatch
- Unstable physical link

**Solution:**
```
/interface pppoe-client set [find name=pppoe-out1] keepalive-timeout=30 mtu=1480 mru=1480
```

### Issue: Slow Internet Speed
**Diagnostic Steps:**
1. Test with bandwidth-test tool
2. Check for queue limits
3. Verify CPU usage
4. Look for packet loss
5. Check for firewall rules causing overhead

### Issue: Customer Can't Access Login Page
**Check:**
```
/ip hotspot walled-garden print
/ip dns print
/ip firewall nat print where chain=dstnat
```
- Ensure walled-garden allows DNS
- Verify DNS server responding
- Check for redirect rules

### Issue: High Latency Spikes
**Causes:**
- CPU overload
- Queue saturation
- Wireless interference
- Bufferbloat

**Solution:**
- Enable PCQ queue type
- Adjust queue limits
- Check wireless CCQ

## Scripting Examples

### Auto-Reconnect Monitor
```
:local pppoeStatus [/interface pppoe-client get [find name=pppoe-out1] running]
:if ($pppoeStatus = false) do={
  /interface pppoe-client disable [find name=pppoe-out1]
  :delay 5s
  /interface pppoe-client enable [find name=pppoe-out1]
  :log info "PPPoE auto-reconnect triggered"
}
```

### Bandwidth Alert
```
:local rxRate [/interface get [name=ether1] rx-byte]
:delay 1s
:local rxRateNew [/interface get [name=ether1] rx-byte]
:local speed (($rxRateNew - $rxRate) / 1024)
:if ($speed > 90000) do={
  :log warning "High bandwidth usage detected: $speed KB/s"
}
```

## Best Practices

1. **Always backup config before changes:**
   ```
   /system backup save name=before-change
   /export file=before-change
   ```

2. **Use safe mode for remote changes:**
   - Press `Ctrl+X` to enter safe mode
   - Changes auto-rollback if disconnected

3. **Monitor logs continuously:**
   ```
   /log print follow
   ```

4. **Document all customer changes**

5. **Test connectivity after every change**

## Security Checklist

- [ ] Default password changed
- [ ] Winbox port changed from 8291
- [ ] SSH using key authentication only
- [ ] Firewall blocking external winbox access
- [ ] Neighbor discovery disabled on WAN
- [ ] UPnP disabled unless required
- [ ] Services limited to necessary ports
