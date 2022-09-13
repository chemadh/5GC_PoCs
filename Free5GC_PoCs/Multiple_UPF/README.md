# Multiple UPF PoC

The objective of this PoC is demonstrating the following 5G functionalities using the Free5GC implementation:
-	Split of user and control plane functions in different VMs and physical hosts. It demonstrates the possibility of using dedicated resources to each type of traffic.
-	Definition of one I-UPF to act as an Uplink Classifier. 
-	Alignment of UERANSIM, UDM and SMF configuration to enable two Data Networks: Internet and IMS. The uplink classifier will address different UPFs to connect with each DN.
-	Split of access and 5GC in different environments interconnected via the VXLAN overlay network.
The overall concept is described in the next diagram:
<img src="./capture_01.PNG" title="./capture_01.PNG" width=700px></img>
 width=700px
