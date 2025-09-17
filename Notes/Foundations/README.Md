
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
They act as the bridge between hardware and applications, allowing user-space programs to interact with devices safely and efficiently.
Examples include drivers for keyboards, mice, GPUs, and network adapters.


