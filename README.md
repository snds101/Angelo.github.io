# [Automotive] 
## A critical zero-click RCE Vulnerability in the in-vehicle infotainment system, the Alpine Halo9 iLX-F509, CVE-2024-23923 ##

In his presentation at Hexacon 2024, Mikhail Evdokimov, a Senior Security Researcher at PCAutomotive, detailed a critical zero-click remote code execution (RCE) vulnerability in the Alpine Halo9 iLX-F509 in-vehicle infotainment (IVI) system. This vulnerability, identified as CVE-2024-23923, resides in the device's proprietary Bluetooth stack and can be exploited without any user interaction. 

Discovery and Analysis:

The research commenced with extractng the firmware from the device's eMMC chip, facilitated by the presence of debug symbols in the production firmware. This enabled a comprehensive reverse-engineering of the Bluetooth stack, leading to the identification of a use-after-free (UAF) vulnerability within the Host Controller Interface (HCI) Asynchronous Connection-Oriented Logical Link (ACL) handling code. Notably, this flaw can be triggered before Bluetooth authentication, broadening the attack surface significantly. 

Exploitation Strategy:

Evdokimov developed a sophisticated exploitation approach, achieving a 96% success rate. Key techniques included:

  * Universal Heap Spraying: Manipulating the heap memory layout to favor successful exploitation.
  
  * Arbitrary Address Write (AAW): Utilizing Enhanced Retransmission Mode (ERTM) channel I-frames to perform arbitrary heap writes, even before authentication, by exploiting a heap overflow condition.
  
  * Heap Segment Memory Leak: Employing Logical Link Control and Adaptation Protocol (L2CAP) echo request/response packets to leak heap segment addresses, facilitating memory structure analysis.
  
  * Arbitrary Address Read (AAR): Exploiting ERTM channel S-frame reject protocol data units (REJ PDUs) to modify heap pointers, enabling reading of arbitrary memory locations.
  
  * Return-Oriented Programming (ROP) Chain Creation: Constructing an ROP chain on the stack to execute system-level commands.
  
  These methods culminated in a highly reliable zero-click RCE exploit. 

Security Implications:

This vulnerability underscores the risks associated with proprietary implementations of standard technologies like Bluetooth. The absence of essential security mitigations—such as stack canaries, Address Space Layout Randomization (ASLR), and the inclusion of debug symbols in production firmware—rendered the system particularly susceptible to exploitation. The vendor's decision to "share the risk" by not releasing a patch highlights the necessity for adopting secure, industry-standard protocols and robust cybersecurity measures in automotive systems.
