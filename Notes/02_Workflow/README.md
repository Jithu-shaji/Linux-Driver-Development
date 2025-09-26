# Linux Kernel Module Development: Workflow

Loadable Kernel Modules (LKMs) provide a mechanism to extend the functionality of the Linux kernel without requiring a full recompilation.  
This workflow illustrates the lifecycle of a simple kernel module, beginning with the build system.

---

## 1. Build System

Kernel modules are **not compiled** in the same way as ordinary user-space programs (e.g., using `gcc`).  
Instead, they must be compiled **in alignment with the Linux kernel’s build system** to ensure proper version compatibility, symbol resolution, and linking.

Linux employs the **Kbuild system**, which is an extension of GNU Make.  
Kbuild simplifies module compilation by allowing developers to specify only the object files that should be built, while the system automatically manages:

- Appropriate compiler flags
- Kernel header file inclusion (`include/linux/*`)
- Linking directives
- Compatibility with the currently running kernel version


example `Makefile`

```makefile
obj-m := hello.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```
- `obj-m := hello.o`
Instructs Kbuild to compile hello.c into hello.o and link it as a loadable kernel module (hello.ko).

  The `.o` file is the intermediate object file; the kernel build system transforms it into a `.ko` (kernel object).

- `make -C /lib/modules/$(shell uname -r)/build`
Directs make to enter the kernel build directory associated with the currently running kernel.

  This ensures that the module is built against the exact kernel version in use.

- `M=$(PWD)`
Specifies that the module’s source code is located in the present working directory.
  
  Kbuild will look here for the Makefile and module source files.
- `modules`
Builds all modules declared by obj-m.


## 2. Module Source Code

Kernel modules must have at least two functions: a `start` function  which is called when the module is inserted in to 
the kernel `insmod`, and an `end` function which is called just before it is removed from kernel using `rmmod`.
---
Here is an example  `hello.c` to showcase the basic structure of a kernel module.
```bash
#include <linux/module.h>   /* Needed by all modules */
#include <linux/kernel.h>   /* Needed for KERN_INFO */   

static int hello_init(void) {
    printk(KERN_INFO "Hello, Kernel World!\n");
    return 0;
}

static void hello_exit(void) {
    printk(KERN_INFO "Goodbye, Kernel World!\n");
}

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Your Name");
MODULE_DESCRIPTION("A simple Hello World kernel module");
```

- `#include <linux/module.h>` : 
Required for all kernel modules. Defines core module macros and functions like `module_init`, `module_exit`, and `MODULE_LICENSE`.


- `#include <linux/kernel.h>` : 
Provides macros like `printk()` and log levels (`KERN_INFO`, `KERN_ERR`, etc.) used for kernel logging.


