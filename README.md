---

# Packet Tracer Lab: HSRPv2 Configuration and External Ping

## Lab Overview
This lab demonstrates the configuration of **Hot Standby Router Protocol Version 2 (HSRPv2)** on two routers (R1 and R2), setting up a virtual IP (VIP) as the default gateway for a local network. The lab tests the failover functionality by pinging an external server from PCs in the local network and observing changes when one of the routers is turned off and back on.

<img src= "https://github.com/ro-drick/HSRP-Configuration/blob/main/hsrp.PNG">

### Topology Details:
- **Default Gateway (VIP):** 10.0.1.254
- **HSRP Configuration:**
  - **R1 Priority:** 200
  - **R2 Priority:** 50
- **External Server to Ping:** 8.8.8.8
- **Default Gateway on PCs:** 10.0.1.253 (before VIP is configured)

## Objectives

1. **Ping External Server (8.8.8.8) from PC1/PC2**
   - The default gateway for both PCs is initially set to **10.0.1.253**.
   - PCs should be able to ping the external server through the network, routing via the HSRP active router.

2. **Configure HSRPv2 on R1 and R2**
   - **R1 Priority:** Increased to 200 to make it the preferred active router.
   - **R2 Priority:** Decreased to 50 to serve as the backup.
   - Enable preemption on both routers so the highest priority router takes over if it becomes available.

3. **Update Default Gateway to VIP**
   - Configure **10.0.1.254** as the new default gateway (VIP) for PC1 and PC2.
   - Verify connectivity by pinging **8.8.8.8** from both PCs.
   - Check the ARP table to confirm the **MAC address** associated with the VIP.

4. **Failover Test (Turn off R1)**
   - Power off **R1** and verify that **R2** takes over as the default gateway.
   - Ping **8.8.8.8** from PC1 to confirm that the network is still operational using **R2** as the gateway.

5. **Restore R1 and Verify Active Role**
   - Power on **R1** and verify if it reclaims its role as the active router.
   - Check that **R1** is the default gateway again by observing traffic and HSRP status.

## Key Findings

- **Default Gateway:** Initially set to **10.0.1.253**, changed to **VIP 10.0.1.254** after HSRP configuration.
- **MAC Address Mapping:** After configuring HSRP, the **VIP 10.0.1.254** is associated with the MAC address generated by the HSRP protocol.
- **Failover Behavior:** When **R1** is powered off, **R2** takes over as the default gateway. Once **R1** is powered back on, it preempts **R2** and becomes the active router again.

## Commands Used

### HSRPv2 Configuration on R1:
```bash
R1(config)# interface g0/1
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 10.0.1.254
R1(config-if)# standby 1 priority 200
R1(config-if)# standby 1 preempt
```

### HSRPv2 Configuration on R2:
```bash
R2(config)# interface g0/1
R2(config-if)# standby version 2
R2(config-if)# standby 1 ip 10.0.1.254
R2(config-if)# standby 1 priority 50
R2(config-if)# standby 1 preempt
```

---

## Conclusion

This lab successfully demonstrated the configuration of HSRPv2, showcasing its capability to provide gateway redundancy and ensure high availability in a network. By raising **R1’s priority** to 200 and lowering **R2’s priority** to 50, we designated R1 as the preferred active router and R2 as the backup. The configuration of a **Virtual IP (VIP) at 10.0.1.254** allowed seamless failover between the routers without disrupting the network's connectivity.

Testing confirmed that both **PC1 and PC2** could maintain connectivity to the external server **8.8.8.8**, even when **R1** was powered off, with **R2** taking over the role of default gateway. Once **R1** was restored, it preempted and became the active router again, validating the effectiveness of preemption in HSRP.

This lab highlights the importance of redundancy in critical network infrastructure, providing uninterrupted service even in the event of device failure. Properly configuring HSRP ensures that network availability and resilience are maintained, offering a robust solution for default gateway redundancy in real-world networks.

---

## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
