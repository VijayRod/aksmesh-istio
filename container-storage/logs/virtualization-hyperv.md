## hyperv

```
root@aks-nodepool1-26613092-vmss000000:/# cat /dev/vmbus/hv_kvp
cat: /dev/vmbus/hv_kvp: Device or resource busy
```

- https://msrc.microsoft.com/blog/2019/01/fuzzing-para-virtualized-devices-in-hyper-v/
- https://www.kernel.org/doc/html/latest/virt/hyperv/index.html
- https://www.red-gate.com/simple-talk/devops/containers-and-virtualization/microsoft-hyper-v-networking-and-configuration-part-1/: the basics of Hyper-V networking
- https://www.red-gate.com/simple-talk/devops/containers-and-virtualization/microsoft-hyper-v-networking-and-configuration-part-2/: The networking configuration or scenarios explained in this 2nd part can be used on Hyper-V running on Windows Server 2008, Windows Server 2008 R2 and Windows Server 2012.
- https://www.red-gate.com/simple-talk/devops/containers-and-virtualization/microsoft-hyper-v-networking-and-configuration-part-iii/: best-practices for Hyper-V Networking and gave a few Hyper-V Networking examples.

## hyperv.virtualswitch.hvnetvsc

- https://learn.microsoft.com/en-us/troubleshoot/azure/virtual-machines/linux-hyperv-issue: When a Hyper-V child partition is started and the guest operating system is running, the virtualization stack starts the Network Virtual Service Client (NetVSC).
- https://learn.microsoft.com/en-us/azure/virtual-network/accelerated-networking-how-it-works: When a virtual machine (VM) is created in Azure, a synthetic network interface is created for each virtual NIC in its configuration. The synthetic interface is a VMbus device and uses the netvsc driver. Network packets that use this synthetic interface flow through the virtual switch in the Azure host and onto the datacenter's physical network.
- https://www.kernel.org/doc/html/latest/networking/device_drivers/ethernet/microsoft/netvsc.html
- https://learn.microsoft.com/en-us/troubleshoot/azure/virtual-machines/linux-hyperv-issue

## hyperv.virtualswitch.srvio

- https://learn.microsoft.com/en-us/azure/virtual-network/accelerated-networking-overview: Accelerated Networking enables single root I/O virtualization (SR-IOV) on supported virtual machine (VM) types, greatly improving networking performance. This high-performance data path bypasses the host
- https://azure.microsoft.com/en-us/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/: much of Azure's software-defined networking stack off the CPUs and into FPGA-based SmartNICs
- https://learn.microsoft.com/en-us/azure/virtual-network/accelerated-networking-mana-overview: In instances where the operating system doesn't or can't support MANA, network connectivity is provided through the hypervisor’s virtual switch.
- https://learn.microsoft.com/en-us/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-

## hyperv.VMBus

- https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/reference/hyper-v-architecture: The VMBus is a logical inter-partition communication channel. The parent partition hosts Virtualization Service Providers (VSPs) which communicate over the VMBus to handle device access requests from child partitions...
- https://www.kernel.org/doc/html/latest/virt/hyperv/vmbus.html: VMBus is a software construct provided by Hyper-V to guest VMs. 
- https://github.com/torvalds/linux/blob/master/Documentation/virt/hyperv/vmbus.rst


## hyperv.VMBus.child-partition[].IntegrationServices (VMware Tools)

```
# See the section on VSP
```

## hyperv.VMBus.child-partition[].IntegrationServices.VSC

```
# See the section on VSP
```

- https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/reference/hyper-v-architecture: Integration components, which include virtual server client (VSC) drivers, are also available for other client operating systems.

## hyperv.VMBus.parent-partition.IntelVT (AMD-V)

- https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/reference/hyper-v-architecture: Hyper-V requires a processor that includes hardware assisted virtualization, such as is provided with Intel VT or AMD Virtualization (AMD-V) technology.

## hyperv.VMBus.parent-partition.VSP

- https://www.serverwatch.com/guides/understanding-hyper-v-vsp-vsc-and-vmbus-design/: Hyper-V implements Server-Side and Client-Side components called VSP and VSC, respectively. VSP stands for Virtualization Service Provider and VSC stands for Virtualization Service Clients
  - In a virtual environment, operating system components send hardware access requests using native drivers, but the requests are received by the virtual layer. Such requests are intercepted by the virtual layer before the request for accessing the hardware devices is honored. This interception is sometimes referred to as device emulation.
  - To avoid the extra layer of communication (device emulation), Microsoft provides a set of components called “Integration Services” for virtual machines running on Hyper-V. VMware provides “VMware Tools” for virtual machines running on ESX Server.
  - These two components (VSP and VSC) help facilitate the smooth and reliable communication between Child Partitions (Virtual Machines) and the Parent Partition (Hyper-V Server). VSPs always run in Parent Partition and VSCs run in Child Partitions.
  - There are four VSPs (Network, Video, Storage and HID) available in Hyper-V as well as four VSCs running in multiple child partitions
  - Both VSPs and corresponding VSCs communicate with each other using a communication channel called VMBUS. VMBUS is a special protocol designed to pass communication from a VSC to VSP running in the Parent Partition.
  - There can only be four VSPs running on a Hyper-V Server in the Parent Partition, but there could be multiple VSCs running on the same Hyper-V Server as part of the child partitions (four in each child partition).
  - VSPs are multithreading components that are running as part of the VMMS.exe and can serve multiple VSCs requests simultaneously.
- https://techcommunity.microsoft.com/blog/coreinfrastructureandsecurityblog/hyper-v-integration-services-where-are-we-today/259554: Services running in the Guest/Host