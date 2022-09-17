# Proofs of concept configurations for open-source 5GCs

The technology disruption driven by 5G standards justifies the necessity of environments where practical implementation and review of the advanced set of functionalities introduced. Additionally, some of the 5G functionalities requires a high level of abstraction that is difficult to achieve without a practical view.

On this regard, there are two open-source implementations of 5G Core that could be uses as basis for these Proofs of Concepts: Free5GC (https://www.free5gc.org/) and Open5GS (https://open5gs.org/). This repository organize the available examples in one subdirectory for each initiative:
- Free5GC_PoCs: https://github.com/chemadh/5GC_PoCs/tree/main/Free5GC_PoCs
- Open5GS_PoCs: https://github.com/chemadh/5GC_PoCs/tree/main/Open5GS_PoCs

In addition to this, UERANSIM (https://github.com/aligungr/UERANSIM) is a simulation tool to emulate UEs and gNBs using software components, to be connected to 5GCs.  It is required to avoid the setup complexity of SDR radios, terminal and SIM configurations, since the focus of the activity is in 5G Core functionalities.

## Virtualized test environment

The environment used to deploy the presented PoCs is Proxmox Virtual Environment (PVE), and it has been deployed in two clusters: one compound by 3 local physical machines, and other deployed in two VPS equipment in a public cloud. The two environments are configured with a common VXLAN overlay network to share the same L2 networking space. Anyway, any other virtualized environment is suitable to build similar examples meanwhile it includes local LAN connectivity between virtualized elements (i.e. OpenStack, virtualBox, etc).

## Additional references

The following list contains third repositories with very valuable example configurations of Free5GC and Open5GS:
- Shigeru Ishida's github respositories: https://github.com/s5uishida?tab=repositories
