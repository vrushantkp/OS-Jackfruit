# Multi-Container Runtime 

## 🔹 Team Information

* **Vrushant K P** — PES1UG24CS542
* **Yashas Shivrajappa** — PES1UG24CS545

---

## 🔹 Overview

This project implements a lightweight container runtime in C using Linux system primitives. It demonstrates core operating system concepts such as process isolation, scheduling, logging, and kernel interaction.

### 🔧 Key Features

* Multi-container execution using namespaces
* Centralized supervisor process
* Per-container logging system
* Kernel-level monitoring using a Loadable Kernel Module (LKM)
* CPU vs I/O scheduling demonstration
* Clean lifecycle management

---

## 🔹 Build, Load, and Run Instructions

### 1. Build the Project

```bash
cd boilerplate
make
```

---

### 2. Load Kernel Module

```bash
sudo insmod monitor.ko
sudo dmesg | tail -n 20
```

---

### 3. Start Supervisor

```bash
sudo ./engine supervisor ./rootfs-base
```

---

### 4. Prepare Containers

```bash
cp -a ./rootfs-base ./rootfs-alpha
cp -a ./rootfs-base ./rootfs-beta
```

---

### 5. Start Containers

```bash
sudo ./engine start alpha ./rootfs-alpha /cpu_hog
sudo ./engine start beta ./rootfs-beta /io_pulse
```

---

### 6. Inspect Containers

```bash
sudo ./engine ps
```

---

### 7. View Logs

```bash
ls logs
cat logs/alpha.log
```

---

### 8. Stop Containers

```bash
sudo ./engine stop alpha
sudo ./engine stop beta
```

---

##  Demo with Screenshots

### 1. Build Process

Compilation of the runtime and kernel module.
![Build](build.png)

---

### 2. Supervisor Execution

Supervisor initializing and managing containers.
![Supervisor](supervisor.png)

---

### 3. Multi-Container Execution

Running multiple containers (alpha & beta).
![Containers](containers.png)

---

### 4. Container Metadata (PS Output)

Displays container ID, PID, and state.
![PS](ps.png)

---

### 5. Logging System

Per-container logs being generated.
![Logs](logs.png)

---

### 6. Kernel Module Output

Kernel module successfully loaded and messages observed via dmesg.
![Kernel](dmseg.png)

---

### 7. CPU Scheduling Behavior

CPU-bound process visible using `top`.
![Top](top.png)

---

## 🔹 Engineering Analysis

### 🔹 Process Isolation

* Containers use Linux namespaces
* Separate process trees and environments
* Isolated execution

---

### 🔹 Supervisor Design

* Central controller for container lifecycle
* Handles creation and termination
* Coordinates logging

---

### 🔹 Logging System

* Each container logs independently
* File-based logging
* Persistent output after execution

---

### 🔹 Kernel Monitoring

* Implemented using LKM
* Demonstrates kernel-user interaction
* Tracks container-related activity

---

### 🔹 Scheduling Behavior

* CPU-bound processes use high CPU
* I/O-bound processes yield CPU
* Demonstrates Linux scheduler

---

## 🔹 Design Decisions & Tradeoffs

### Container Isolation

* Namespace-based approach
* Lightweight but less secure than full container runtimes

### Supervisor

* Single controller process
* Easy to manage but single point of failure

### Logging

* File-based logs
* Simple but limited scalability

### Kernel Monitoring

* LKM-based tracking
* Powerful but increases complexity

---

## 🔹 Observations

* CPU-intensive tasks dominate CPU usage
* Logging works consistently
* Containers start and stop correctly
* Kernel module loads successfully

---

## 🔹 Notes

* Screenshots captured from a Linux VM
* Some container commands may exit quickly depending on workload

---

## 🔹 Conclusion

This project demonstrates a simplified container runtime integrating:

* Process isolation
* Logging pipeline
* Kernel interaction
* Scheduling behavior

It provides practical insight into core operating system concepts.
