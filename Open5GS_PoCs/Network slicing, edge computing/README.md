# Open5GS - Network slicing, edge computing

The objective of this PoC is demonstrating network slicing and edge computing functionalities using the Open5GS implementation. In order to achieve this, the following setup is deployed in the test workbench:
-	Three instances of Open5GS core in two Proxmox clusters (Local and VPS), and three different locations (Madrid -Spain-, Gravelines -France- and Frankfurt -Germany-).
    - First Open5GS instance in Madrid (LxC in Lserver1 Proxmox host), containing all 5GC components.
    - Second Open5GS instance in Gravelines (LxC in VPS1 Proxmox host), containing UPF, SMF and AMF.
    - Third Open5GS instance in Frankfurt (LxC in VPS2 Proxmox host), containing UPF, SMF and AMF.
- Three UERANSIM instances, in the same three environments where Open5GS deployments are available.
- One LxC instance containing an example web server, to be associated to MEC/Edge computing service. It is deployed in Local Proxmox Lserver2 host, and configured to serve the FQDN edgetest.open5gs.com.

This environment is used to demonstrate the following concepts:
-	Each UE in UERANSIM instance available, using the default slice NSSAI and Internet DNN, connects with its local AMF and SMF, and enables the local GTP user plane with the local UPF to reach Internet DNN locally, with the minimum latency possible. It is also enabled by configuring different TACs in each location (in both UERANSIM’s gNB and AMFs).
-	There will be also an alternative slice NSSAI available for UERANSIM instances and Open5GS instance in Madrid. It will enable that UEs using this alternative NSSAI will be served by Madrid Open5GS core network instance, enabling also a local resolution of edgetest.open5gs.com address, that is associated to the local LxC instance where the MEC/Edge example web server is available.

The overall concept is also described in the next diagram:


