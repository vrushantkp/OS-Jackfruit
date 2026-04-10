# Multi-Container Runtime

## 🔹 Team Information

* **Vrushant K P** — PES1UG24CS542
* **Yashas Shivrajappa** — PES1UG24CS545

---

## 🔹 Overview

This project implements a lightweight container runtime in C, inspired by core Linux container principles. It demonstrates how operating system concepts like process isolation, scheduling, and kernel interaction work together in a practical system.

### 🔧 Key Features

* Multi-container execution using Linux namespaces
* Centralized supervisor process
* Per-container logging system
* Kernel-level monitoring using a Loadable Kernel Module (LKM)
* CPU vs I/O scheduling experiments
* Clean lifecycle management (no zombie processes)

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

### 4. Prepare Container Root Filesystems

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

## 📸 Demo with Screenshots

### 1. Multi-Container Supervision

Shows the supervisor launching and managing multiple containers.

![Supervisor](images/supervisor.png)

---

### 2. Container Metadata (CLI)

Displays container ID, PID, and state.

![PS Output](images/containers.png)

---

### 3. Logging System

Each container generates logs stored separately.

![Logs](images/logs.png)

---

### 4. Kernel Monitoring

Kernel module successfully loaded and device initialized.

![Kernel Monitor](images/dmesg.png)

---

### 5. Scheduling Behavior (CPU vs IO)

CPU-bound tasks consume higher CPU compared to I/O-bound tasks.

![Top](images/top.png)

---

### 6. Clean Teardown (No Zombies)

System ensures no zombie processes remain.

![No Zombies](images/zombies.png)

---

## 🔹 Engineering Analysis

### 🔹 Process Isolation

Containers are created using Linux namespaces:

* Separate process trees
* Isolated filesystem environments
* Independent execution contexts

---

### 🔹 Supervisor Design

The supervisor acts as:

* A central controller for container lifecycle
* A process manager for spawning and stopping containers
* A coordinator for logging and monitoring

---

### 🔹 Logging System

* Each container writes to its own log file
* Output is captured via pipes
* Logs persist even after container termination

---

### 🔹 Kernel Monitoring

* Implemented as a Loadable Kernel Module
* Interacts with user-space runtime
* Demonstrates kernel-user communication

---

### 🔹 Scheduling Behavior

* CPU-bound processes utilize maximum CPU
* I/O-bound processes frequently yield CPU
* Demonstrates real-world Linux scheduling behavior

---

## 🔹 Design Decisions & Tradeoffs

### Container Isolation

* **Approach:** Namespace-based
* **Tradeoff:** Lightweight but less secure than full container runtimes

### Supervisor

* **Approach:** Single process controller
* **Tradeoff:** Simple design but single point of failure

### Logging

* **Approach:** File-based logging
* **Tradeoff:** Easy implementation, limited scalability

### Kernel Monitoring

* **Approach:** LKM-based tracking
* **Tradeoff:** Powerful but increases complexity

---

## 🔹 Observations

### CPU vs IO

* `cpu_hog` consumes significantly higher CPU
* `io_pulse` uses minimal CPU

### System Behavior

* Containers execute and terminate cleanly
* Logs are consistently generated
* No zombie processes observed

---

## 📝 Notes

* All outputs captured from a Linux VM environment
* Kernel module successfully loads and initializes
* Some container commands may exit quickly, but system behavior remains consistent

---

## ✅ Conclusion

This project demonstrates a simplified container runtime that integrates:

* Process isolation
* Logging pipeline
* Kernel interaction
* Scheduling behavior

It provides a strong practical understanding of how operating systems manage processes, resources, and execution environments.
