
**User Space**

This is where normal applications run — for example, your C programs, Firefox, or VS Code.
Applications in user space cannot directly access hardware such as the keyboard, GPU, or storage devices.
Instead, they must request services from the kernel.

**Kernel Space**

The kernel operates in a privileged mode with full access to the system’s hardware.
It can directly interact with devices through registers, I/O ports, and memory-mapped I/O.
However, errors in kernel space can be critical, potentially leading to a system crash (kernel panic).

**Device Drivers**

Device drivers reside in kernel space.
They act as the bridge between hardware and applications, allowing user-space programs to interact with devices safely 
and efficiently.
Examples include drivers for keyboards, mice, GPUs, and network adapters.

### What is a Loadable Kernel Module (LKM)?

A Loadable Kernel Module (LKM) is a separately compiled, object-oriented component of the Linux kernel that can be 
dynamically inserted into or removed from the kernel during system operation. LKMs are distributed as kernel object 
files (.ko), which contain code and metadata required for integration with the running kernel. In contrast to the 
statically linked kernel image (vmlinuz), LKMs enable extensibility and modularity by allowing additional 
functionality—such as device drivers, file systems, or networking protocols—to be incorporated without recompiling or rebooting the kernel.


