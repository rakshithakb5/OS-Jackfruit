# OS Jackfruit Project –Kernel Module Monitoring & Container Supervision

---

# 1. Team Information

Team Members:

* Rakshitha K B – SRN: PES2UG24CS918
* Mythri B H – SRN: PES2UG24CS290

---

# 2. Build, Load, and Run Instructions

## Step 1: Build the Project

make

## Step 2: Load Kernel Module

sudo insmod monitor.ko

## Step 3: Verify Device

ls -l /dev/container_monitor

## Step 4: Start Supervisor

sudo ./engine supervisor ./rootfs-base

## Step 5: Create Writable Root Filesystems

cp -a ./rootfs-base ./rootfs-alpha
cp -a ./rootfs-base ./rootfs-beta

## Step 6: Start Containers

sudo ./engine start alpha ./rootfs-alpha /bin/sh --soft-mib 48 --hard-mib 80
sudo ./engine start beta ./rootfs-beta /bin/sh --soft-mib 64 --hard-mib 96

## Step 7: List Containers

sudo ./engine ps

## Step 8: View Logs

sudo ./engine logs alpha

## Step 9: Run Workloads

(Execute memory or CPU stress programs inside containers)

## Step 10: Stop Containers

sudo ./engine stop alpha
sudo ./engine stop beta

## Step 11: Check Kernel Logs

dmesg | tail

## Step 12: Unload Module

sudo rmmod monitor

---

# 3. Demo with Screenshots

The following demonstrations were performed with screenshots:

1. Multi-container supervision
   → Shows multiple containers running under a single supervisor

2. Metadata tracking
   → Output of `engine ps` showing container details

3. Bounded-buffer logging
   → Logs showing producer-consumer behavior

4. CLI and IPC
   → CLI command interaction with supervisor

5. Soft-limit warning
   → dmesg output showing soft memory limit exceeded

6. Hard-limit enforcement
   → dmesg output showing process termination

7. Scheduling experiment
   → Output comparison of workloads

8. Clean teardown
   → No zombie processes after stopping containers

(All screenshots are included in the screenshots folder with captions)

---

# 4. Engineering Analysis

This project demonstrates key operating system concepts including process isolation, scheduling, and kernel-level monitoring.

The kernel module periodically monitors memory usage of processes using RSS values. Soft and hard limits are enforced to detect abnormal behavior. The supervisor manages multiple containers and coordinates execution using IPC mechanisms.

The project highlights how Linux handles process scheduling and resource allocation. Kernel-level monitoring provides deeper insights compared to user-space tools.

---

# 5. Design Decisions and Tradeoffs

## Namespace Isolation

Choice: Used container-based isolation
Tradeoff: Slight overhead in setup
Justification: Provides process separation and safety

## Supervisor Architecture

Choice: Central supervisor process
Tradeoff: Single point of control
Justification: Easier management of containers

## IPC and Logging

Choice: CLI-based interaction and logging pipeline
Tradeoff: Added complexity
Justification: Enables communication and monitoring

## Kernel Monitor

Choice: Timer-based monitoring
Tradeoff: Periodic overhead
Justification: Efficient and simple implementation

## Scheduling Experiments

Choice: Multiple workload comparison
Tradeoff: Requires controlled setup
Justification: Demonstrates real OS scheduling behavior

---

# 6. Scheduler Experiment Results

Experiments were conducted by running workloads across multiple containers.

Observations:

* CPU-intensive tasks showed higher execution time under contention
* Memory-heavy workloads triggered soft and hard limits
* Scheduling behavior varied based on workload distribution

Sample Comparison:

Workload A vs Workload B

* A: Faster completion under low load
* B: Slower under contention

Conclusion:
The results demonstrate how Linux scheduling dynamically allocates CPU resources based on process demand and system load.

---

# Conclusion

Task 6 successfully demonstrated kernel-level monitoring and multi-container supervision. The project provided practical exposure to OS internals, process scheduling, and resource management.
