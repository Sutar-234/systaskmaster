# Stage 1: Project Introduction & Scope Definition

## Project Title
**Linux Kernel-Assisted Resource & Task Management System (SysTaskMaster)**

---

## 1. Project Idea & Objective
**Linux Kernel-Assisted Resource & Task Management System (SysTaskMaster)** is a hybrid systems engineering tool designed to monitor system resources, manage active process lifecycles, and coordinate hardware-level metrics between user space and kernel space in real time.

The primary objective is to implement a unified, production-grade monitoring and task governance suite demonstrating core concepts from systems engineering:
1. **Computer Systems & Architecture:** Tracking CPU state cycles (user, system, idle), memory hierarchy usage (physical RAM versus swap), and hardware context-switch counters[cite: 1].
2. **Operating System Internals:** Directly reading live system data from the `/proc` virtual filesystem (`/proc/stat`, `/proc/meminfo`, `/proc/[pid]/stat`) without third-party dependencies[cite: 1].
3. **Systems Programming & Daemons:** A persistent background service (`systaskmasterd`) implementing standard POSIX daemonization: a double-fork sequence, session detachment via `setsid()`, file mode creation mask resets (`umask(0)`), and standard stream redirection to `/dev/null`[cite: 1].
4. **Reliable POSIX Signal Handling:** Clean state flushing on `SIGTERM` and `SIGINT`, alongside dynamic configuration reloading on `SIGHUP`, using `sigaction` to eliminate delivery race windows[cite: 1].
5. **Modern C++ Object-Oriented Design:** A clean, polymorphic observer hierarchy (an abstract `TaskObserver` base class with derived `CpuObserver`, `MemoryObserver`, and `ProcessObserver` classes) enforcing encapsulation and RAII resource management[cite: 1].
6. **Network & IPC Layer:** Sockets-based inter-process communication streaming telemetry from the background daemon to interactive client terminals[cite: 1].
7. **Linux Device Driver Integration:** A Loadable Kernel Module (LKM) exposing a character device (`/dev/systask_dev`) to record critical resource events in the kernel message buffer via `printk` and support load-time tuning via `module_param`[cite: 1].

---

## 2. Problem Statement
Server environments and embedded Linux systems frequently face resource exhaustion caused by unmonitored processes, memory leaks, and CPU starvation[cite: 1]. Existing enterprise monitoring suites often introduce heavy runtime overhead, require graphic dependencies, or run entirely detached from low-level kernel event mechanisms[cite: 1].

SysTaskMaster solves this problem by providing:
* Direct, low-overhead parsing of operating system metrics directly from kernel interfaces[cite: 1].
* Seamless coordination between an unprivileged monitoring daemon and a privileged kernel-space character device driver[cite: 1].
* Remote administration capabilities over local or network sockets without desktop dependencies[cite: 1].

---

## 3. Project Scope

### In-Scope Deliverables
* **C++ Telemetry Engine:** An object-oriented library providing structured classes for gathering CPU, memory, and task statistics[cite: 1].
* **Background Daemon (`systaskmasterd`):** A detached POSIX background daemon maintaining scheduling intervals and telemetry history[cite: 1].
* **Process Management Interface:** An administration module to inspect running tasks, adjust task scheduling priority (`nice`/`renice`), and signal rogue processes (`kill`)[cite: 1].
* **Client CLI (`systask-cli`):** A command-line client communicating with the daemon over local POSIX sockets[cite: 1].
* **Kernel Character Device Driver (`systask_driver.ko`):** A companion Linux kernel module supporting dynamic configuration through `module_param` and kernel logging via `printk`[cite: 1].

### Out-of-Scope (Future Enhancements)
* Distributed multi-node server cluster aggregation.
* Web-based browser dashboard rendering.

---

## 4. Expected Outcome & Applications
1. **Headless Server Management:** Real-time visibility into CPU spikes, memory leaks, and thread states on resource-constrained servers.
2. **Embedded Linux Optimization:** Lightweight footprint suitable for headless IoT appliances and embedded development boards[cite: 1].
3. **Automated Resource Governance:** Programmable threshold alerts capable of detecting runaway processes and managing CPU contention[cite: 1].
