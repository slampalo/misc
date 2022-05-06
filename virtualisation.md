
## **Table of Contents**

1. [5 levels of virtualisation](#5_levels_of_virtualisation )
    - [Operating System Level](#operating_system_level)
7. [References](#references)


# 5 levels of virtualisation
## 1. Instruction Set architecture (ISA)

<!-- TODO -->

---

## 2. Hardware Abstraction Level (HAL)
    - Virtualise hardware components such as I/O devices, processors, memory, etc
    - Virtual machines

<!-- TODO -->
---

## 3. Operating System Level
- Abstract layer between the OS and the applications
- containerisation

### **Cgroup (Control group)**
- manage (mainly limit, isolate and measure) resource usage of processes
- groups can be hierarchical

### **Name spaces**
- Separate groups of processes

1. PID (Process ID)
2. Network
3. UTS (UNIX Time-Sharing)
4. Mount
5. IPC (Interprocess Communication)
6. User
7. Cgroup

<!-- TODO -->

---

## 4. Library Level

<!-- TODO -->

---

## 5. Application Level
- Only one program run inside an isolated container
- Isolation of file system

<!-- TODO -->

---

## References:
- [What is a kernel](https://www.ionos.com/digitalguide/server/know-how/what-is-a-kernel/)
- [W3 School - Cloud computing](https://www.w3schools.in/cloud-computing/cloud-virtualization)
- [IBM - Docker](https://www.ibm.com/cloud/learn/docker)
- [chroot command](https://www.geeksforgeeks.org/chroot-command-in-linux-with-examples/?ref=lbp)
- [Kubernetes vs Docker](https://www.geeksforgeeks.org/kubernetes-vs-docker/?ref=leftbar-rightbar)