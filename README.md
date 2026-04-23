# Kamailio SBC: Microsoft Teams ↔ On-Prem VoIP Interconnect

## Overview

This project is a **working prototype of a Session Border Controller (SBC)** built using Kamailio and RTPEngine to enable **Microsoft Teams Direct Routing integration with an on-premises VoIP/PBX system**.

It was developed to **validate interoperability, call routing, and media handling** between a cloud UC platform and a traditional VoIP environment.

The implementation successfully handles:
- SIP signaling over TLS (Teams ↔ SBC ↔ PBX)
- Media interworking (SRTP ↔ RTP)

while highlighting the **core challenges of real-world VoIP integration**, such as NAT traversal, codec handling, and SDP negotiation.

The solution handles both:

* **SIP signaling over TLS** (Teams ↔ SBC ↔ PBX)
* **Media interworking (SRTP ↔ RTP)** via RTPEngine

It reflects a **real-world interoperability scenario** between cloud UC platforms and traditional VoIP infrastructure.

---

## Key Capabilities

* **SIP Routing & Call Control**

  * Bidirectional call routing between Microsoft Teams and on-prem PBX
  * Stateful dialog handling and in-dialog request processing

* **SBC Functionality**

  * TLS termination for Teams Direct Routing
  * Topology hiding and SIP normalization
  * Dispatcher-based failover across Microsoft PSTN hubs

* **Media Handling (RTPEngine)**

  * SRTP ↔ RTP interworking
  * SDP rewriting (origin, connection)
  * ICE handling for Teams compatibility
  * Codec handling (e.g., PCMA)

* **NAT Traversal**

  * Media anchoring via RTPEngine
  * Correct handling of public/private IP addressing

---

## Architecture

* **Kamailio**

  * Acts as the SBC (signaling layer)
  * Routes calls between Teams and PBX
  * Applies routing logic and header manipulation

* **RTPEngine**

  * Anchors and transforms media streams
  * Handles SRTP (Teams) and RTP (PBX)

* **Microsoft Teams (Direct Routing)**

  * SIP over TLS via `sip.pstnhub.microsoft.com`

* **On-Prem PBX (Simulated)**

  * Represents a legacy VoIP system (e.g., Asterisk/FreePBX)

---

## Call Flow (High Level)

### Teams → PBX

1. SIP INVITE received from Microsoft PSTN hub (TLS)
2. Kamailio routes request to PBX
3. RTPEngine converts SRTP → RTP
4. Media is anchored and delivered to PBX

### PBX → Teams

1. SIP INVITE received from PBX
2. Routed via Kamailio to Microsoft PSTN hub (TLS)
3. RTPEngine converts RTP → SRTP
4. ICE enforced for Teams compatibility

---

## Repository Layout

| Path                                                                                | Description                                                                    |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [playbook.yaml](playbook.yaml)                                                      | Ansible playbook used to deploy Kamailio, RTPEngine, and supporting components |
| [roles/kamailio-install/](roles/kamailio-install/)                                  | Core role: installs Kamailio, RTPEngine, and deploys configuration templates   |
| [templates/kamailio.cfg.j2](roles/kamailio-install/templates/kamailio.cfg.j2)       | Main SIP routing logic (Teams ↔ PBX), call handling, and RTP control           |
| [templates/dispatcher.list.j2](roles/kamailio-install/templates/dispatcher.list.j2) | Microsoft PSTN hub endpoints and failover configuration                        |
| [templates/rtpengine.conf.j2](roles/kamailio-install/templates/rtpengine.conf.j2)   | Media plane configuration (interfaces, RTP ports, bridging)                    |
| [templates/tls.cfg.j2](roles/kamailio-install/templates/tls.cfg.j2)                 | TLS configuration for secure SIP (Teams Direct Routing)                        |
| `roles/asterisk-*`, `roles/freepbx-*`                                               | Optional components used to simulate an on-prem PBX environment                |

---

## Example SBC Logic

* Traffic classification (Teams vs PBX origin)
* Dynamic routing using destination URI (`$du`)
* Media policy enforcement:

  * SRTP toward Teams
  * RTP toward PBX
* TLS configuration with SNI for Microsoft endpoints

---

## Notes

* Simplified for clarity and experimentation
* Certificates and credentials are placeholders and must be replaced for real deployments

