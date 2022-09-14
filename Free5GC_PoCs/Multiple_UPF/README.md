# Multiple UPF PoC

The objective of this PoC is demonstrating the following 5G functionalities using the Free5GC implementation:
-	Split of user and control plane functions in different VMs and physical hosts. It demonstrates the possibility of using dedicated resources to each type of traffic.
-	Definition of one I-UPF to act as an Uplink Classifier. 
-	Alignment of UERANSIM, UDM and SMF configuration to enable two Data Networks: Internet and IMS. The uplink classifier will address different UPFs to connect with each DN.
-	Split of access and 5GC in different environments interconnected via the VXLAN overlay network.
The overall concept is described in the next diagram:

<img src="./capture_01.PNG" title="./capture_01.PNG" width=700px></img>

## Detail of resources deployed in the test bench to enable this configuration
-	First Free5GC VM: It runs all 5GC nodes, except UPF, to handle all control plane functions.
-	Second Free5GC VM: instance containing UPF that handles Internet DN.
-	Third Free5GC VM: instance containing UPF that handles IMS DN.
-	Fourth Free5GC VM: It runs the I-UPF acting as Uplink Classifier. It connects the user plane with UERANSIM gNB in one side, and Internet or IMS dedicated UPFs, depending on the DN traffic.

## Additional elements required to build the PoC
-	UERANSIM LxC instance to simulate the access network (gNB) and UE.
-	LxC instance to simulate the Internet Gateway function (simple Linux VM)
-	LxC instance to simulate the IMS node (simple Linux VM)
Distribution of elements in Poxmox local and VPS clusters’ test bench:
	- UERANSIM in VPS1 physical node (VPS cluster)
	- Free5GC - control plane: lserver1 physical node (local cluster)
	- Free5GC - UPFs: lserver2 physical node (local cluster)
	- LxCs internet gateway y IMS, lserver3 physical node (local cluster)
  
## Low-level details of configuration applied:
- First Free5GC VM:
	- Update of amfcfg.yaml for AMF configuration, setting the N2 interface in ngapIpList element and Internet and DNNs in supportDnnList element (complete file in https://github.com/chemadh/5GC_PoCs/tree/main/Free5GC_PoCs/Multiple_UPF/config_files/free5gc/amfcfg.yaml)
	- Update of smfcfg.yaml for SMF configuration, setting the following ((complete file in https://github.com/chemadh/5GC_PoCs/tree/main/Free5GC_PoCs/Multiple_UPF/config_files/free5gc/smfcfg.yaml):
		- IMS and Internet DNN in dnnInfos section of sNssai defined in snssaiInfos section, where supported S-NSSAIs are defined.
		- N4 interface definition in pfcp section.
		- Definition of UPF-I, UPF-Internet and UPF-IMS associations in up_nodes element of userplane_information section.
		- Definition of user plane topology in links section.
	- Update uerouting.yaml to identify routing details related to UEs, where the user plane topology defined in SMF config is replicated and associated to UEs to be used in the test (complete file in https://github.com/chemadh/5GC_PoCs/tree/main/Free5GC_PoCs/Multiple_UPF/config_files/free5gc/uerouting.yaml).
	- Update run.sh to not launch UPF in this VM (complete file in https://github.com/chemadh/5GC_PoCs/tree/main/Free5GC_PoCs/Multiple_UPF/config_files/free5gc/run.sh).
	- Update the configuration of subscriber information to use in the web UI, to add an IMS DNN. A capture of the resulting change can be seen below:

<img src="./capture_02.PNG" title="./capture_02.PNG" width=400px></img>

- Second Free5GC VM:
		- Update of upfcfg.yaml for UPF configuration, setting pfcp and gtpu addresses in their relative sections. Definition of Internet in dnn_list section.
- Third Free5GC VM:
	- Update of upfcfg.yaml for UPF configuration, setting pfcp and gtpu addresses in its respective sections. Definition of IMS in dnn_list section.
- Fourth Free5GC VM:
	- Update of upfcfg.yaml for UPF configuration, setting pfcp and gtpu addresses in its respective sections. Definition of IMS and Internet in dnn_list section.
- Global reconfiguration of startup script in each Free5G VM (run.sh):
	- For VMs running only UPF, removal of parts of the script dedicated to launch the rest of 5GC elements.
	- For VM running only the control plane, removal of parts of the script dedicated to launch the UPF.
- Update of 5GC subscription information (UDM web UI): Update of one of the UEs to define two DNNs: Internet and IMS.
- Update of UERANSIM: update of UE configuration file (i.e. freegc_ue.yaml), to define both Internet and IMS DNNs.

## Tests done:
Once all Free5GC and UERANSIM components are running, it can be checked that UERANSIM node has raised two GTP tunnels with I-UPF, one for Internet DNN and other for IMS DNN.
Connectivity tests (i.e. ping with tcpdump traffic validation) can be done to verify that:
-	Traffic connecting with Internet Gateway and using GTP interface associated to Internet DNN in UE, follows the user plane path gNB – i-UPF – Internet UPF – Internet Gateway LxC.
-	Traffic connecting with IMS node and using GTP interface associated to IMS DNN in UE, follows the user plane path gNB – i-UPF – IMS UPF – IMS LxC.

## Possible further improvements:
In addition to the present PoC definition, the following improvements that can be identified for further activities (out of the scope of the present Master Thesis):
1.	Deployment of Kamailo IMS (included since Kamailio 4.0) in IMS LxC instance.
2.	Deployment of a second URANSIM with an additional UE, using also both Internet and IMS DNs. 
3.	Configuration of a SIP phone in each UERANSIM LxC instance, to register two users in Kamailio IMS once the UEs are registered and IMS data session are set up.
4.	Call example between the two UEs. 
