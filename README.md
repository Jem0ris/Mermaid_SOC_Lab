# Mermaid SOC Lab

I built a home‑made Security Operations Center lab that imitates a hospital network. I used GNS3 to create attack, detection and mitigation cycles for every threat that I simulated. This project is part of my hands‑on cybersecurity portfolio.

## Scenario

This lab represents a made‑up hospital network. It has an edge firewall, a LAN that is watched and endpoint devices that send data to a SIEM. The goal is to show how attackers and defenders work in a healthcare IT setting.

## Topology

- **OPNsense-Edge-1** (QEMU). This is the edge firewall and router. It connects to the WAN through GNS3 NAT. Owns the LAN address `192.168.1.1/24`.

- **Switch1**. A unmanaged Layer 2 switch built into GNS3.

- **Parrot-1** (`192.168.1.161`). This machine runs a Wazuh agent. Acts as a general endpoint.

- **Ubuntu-1** (`192.168.1.145`). This machine hosts the Wazuh manager, indexer and dashboard. It is the SIEM.

- **kali-linux-1** (`192.168.1.140`). This machine is the attacker host.

```
                 [GNS3 NAT]
                      |
              [OPNsense-Edge-1]
                      |
                 [Switch1]
            ____________|____________
           |            |            |
      [Parrot-1]   [Ubuntu-1]   [kali-linux-1]
       (agent)      (SIEM)       (attacker)
```

## Objectives

- Show common network attacks that happen in a segment watched by the SOC.

- Confirm that Wazuh, the SIEM, detects attacks when it can.

- Install and test defenses when the lab design allows.

- Explain limitations honestly when the simulation cannot fully show an attack or defense instead of giving false or incomplete results.

## Attacks Covered

# | Attack | Layer | Status | Summary |

|---|--------|-------|--------|---------|

| 1 | SSH Brute Force (Hydra) | Application/Transport Complete | Wazuh noticed the attack and fail2ban stopped it.

| 2 | ARP Spoofing | Layer 2 | Complete | Most ARP replies were fake when checked with tcpdump. A static ARP entry fixed the problem. |

| 3 | MAC/CAM Table Flooding | Layer 2 Complete | No overflow happened. This is a limitation of the GNS3 switch. |

| 4 | DHCP Starvation | Layer 2/3 | Complete | All 150 fake clients got IP offers. A managed switch would help,. It is not in this lab. |

5 | DHCP Spoofing | Layer 2/3 | Complete | A DHCP server fought with the real one. The servers offer was still used. The missing defense is noted. |

6 | TCP SYN Flood | Layer 4 | Planned |. |

| 7 | Nmap Port Scan (reconnaissance) Layer 3/4 | Planned |. |

A full technical breakdown of each attack, evidence and mitigation is provided in the accompanying report (`docs/report.pdf`).

## Known Limitations

- VLAN hopping is a risk,. I cannot show it because the lab lacks a VLAN‑capable switch.

- The GNS3 switch does not have a CAM table, so overflow cannot be shown.

- DHCP snooping needs a managed switch, which this lab does not have. I note this of simulating it.

## Scope and Timeline

I finished this project in a time. I focused on two Layer 3/4 attacks (SYN flood and port scan) because SOC teams usually prioritize the likely threats instead of trying every attack.

## Repository Structure
