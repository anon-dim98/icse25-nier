Artifact for Paper 'A Unit Proofing Framework for Code-level Verification: A Research Agenda'. 

This repository contains the source code and harness modifications we performed to recreate known vulnerabilities and expose them respectively.
They can be used to reproduce the results from Section III in the ICSE NIER paper.
The patches are in the /patches directory.
They follow a naming covention "cve-type.patch" where cve is the CVE ID and type can be one of [source, harness, Makefile].

Here are the instructions.
1. Clone the FreeRTOS+TCP repo (https://github.com/FreeRTOS/FreeRTOS-Plus-TCP).
2. Checkout the commit bfd85142c432ff7668397d1247f9fbb6bed719aa (This commit represents the state of the repo on Friday, May 10th, 2024 and will be our base for the experiments).
3. For each vulnerability, identify the harness verifying the vulnerable function in the table below.
4. Execute the harness, review any CBMC violationes and verify the vulnerability was not exposed.
5. Recreate the vulnerability by applying the patch (using 'git apply <patch-file>.patch') in cve-source.patch where cve is the CVE-ID.
6. Execute the harness again and verify if the vulnerability is reported amongst the CBMC violations.
7. If the vulnerability was not reported, it implies the provided harness is not sufficient to expose the vulnerability.
8. Apply the patch cve-harness.patch and/or cve-Makefile.patch to modify the harness and/or Makefile.
9. Execute the modified harness. Confirm the vulnerability is now reported by CBMC.

Here is the table showing the affected source file and harness for each vulnerability

| CVE               | Source File Changed                           | Harness File Changed                                                                 |
|-------------------|------------------------------------------------|-------------------------------------------------------------------------------------|
| CVE-2018-16523    | FreeRTOS_TCP_Reception.c                       | test/cbmc/proofs/CheckOptionsOuter/Makefile.json                                     |
| CVE-2018-16524    | FreeRTOS_TCP_Reception.c                       | test/cbmc/proofs/CheckOptions/CheckOptions_harness.c                                 |
| CVE-2018-16525    | FreeRTOS_DNS_Parser.c                          | test/cbmc/proofs/ParseDNSReply/ParseDNSReply_harness.c                               |
| CVE-2018-16527    | FreeRTOS_ICMP.c                                | test/cbmc/proofs/ICMP/ProcessICMPPacket/ProcessICMPPacket_harness.c                  |
| CVE-2018-16600    | FreeRTOS_ARP.c                                 | test/cbmc/proofs/ARP/ARPProcessPacket/ARPProcessPacket_harness.c                     |
| CVE-2018-16603    | FreeRTOS_TCP_IP.c                              | test/cbmc/proofs/parsing/ProcessReceivedTCPPacket/ProcessReceivedTCPPacket_harness.c |


