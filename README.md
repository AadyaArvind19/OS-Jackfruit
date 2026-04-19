# OS Mini Project – Container Runtime with Monitoring

##Team Members:
-Aadya Arvind (PES2UG24CS008)
-Aanya K (PES2UG24CS011)

## 📌 Overview

This project implements a lightweight container runtime using a custom `engine` and `supervisor`. It supports:

* Running isolated containers
* Monitoring CPU and memory usage via a kernel module (`monitor.ko`)
* Logging container activity
* Managing container lifecycle (start, stop, logs, ps)

---

## ⚙️ Prerequisites

* Linux environment (Ubuntu VM recommended)
* GCC and Make installed
* Root (sudo) access

---

## 📁 Project Structure

* `engine` → Container runtime CLI
* `monitor.ko` → Kernel module for monitoring
* `cpu_hog` → CPU-intensive workload
* `memory_hog` → Memory-intensive workload
* `rootfs/` → Container filesystem

---

## 🚀 Execution Steps

---

### 🔹 Step 1: Navigate to Project Directory

```bash
cd ~/Desktop/OS-Jackfruit/boilerplate
```

---

### 🔹 Step 2: Build the Project

```bash
make clean
make
```

---

### 🔹 Step 3: Load Kernel Module

```bash
sudo insmod ./monitor.ko
```

Verify:

```bash
lsmod | grep monitor
```

---

### 🔹 Step 4: Start Supervisor (IMPORTANT)

```bash
sudo ./engine supervisor ./rootfs
```

⚠️ Keep this terminal running. Do NOT close it.

---

### 🔹 Step 5: Open New Terminal

```bash
cd ~/Desktop/OS-Jackfruit/boilerplate
```

---

### 🔹 Step 6: Start Containers

#### CPU-intensive container:

```bash
sudo ./engine start alpha ./rootfs ./cpu_hog
```

#### Memory-intensive container:

```bash
sudo ./engine start beta ./rootfs ./memory_hog
```

---

### 🔹 Step 7: Check Running Containers

```bash
sudo ./engine ps
```

---

### 🔹 Step 8: View Logs

```bash
sudo ./engine logs alpha
sudo ./engine logs beta
```

---

### 🔹 Step 9: High vs Low Priority (Scheduling Demo)

#### High priority:

```bash
sudo ./engine start high ./rootfs ./cpu_hog --nice -5
```

#### Low priority:

```bash
sudo ./engine start low ./rootfs ./cpu_hog --nice 10
```

Check logs:

```bash
sudo ./engine logs high
sudo ./engine logs low
```

---

### 🔹 Step 10: Memory Limits Demonstration

```bash
sudo ./engine start memtest ./rootfs ./memory_hog --soft-mib 40 --hard-mib 64
```

Check:

```bash
sudo ./engine ps
```

---

### 🔹 Step 11: Stop Containers

```bash
sudo ./engine stop alpha
sudo ./engine stop beta
sudo pkill -f "./engine supervisor"
ps aux | grep defunct  // to check no zombie process
```

Verify:

```bash
sudo ./engine ps
```

---

### 🔹 Step 12: Cleanup

#### Stop supervisor (in its terminal):

Press `Ctrl + C`

#### Remove kernel module:

```bash
sudo rmmod monitor
```

---

## 📊 Expected Outcomes

* Containers run and show in `engine ps`
* Logs show CPU/memory activity
* High priority process runs faster than low priority
* Memory limits are enforced
* Containers transition through states (running → exited)

---

## 🧠 Key Concepts Demonstrated

* Containerization using namespaces
* Process isolation
* CPU scheduling (nice values)
* Memory limits (soft/hard)
* Kernel module interaction
* Logging and monitoring

---

## ⚠️ Important Notes

* Always start **supervisor before containers**
* Always use `sudo` for engine commands
* Use `./` before executables (e.g., `./engine`)
* Logs appear only when workload is running (not for `/bin/sh`)

---

## 🏁 Conclusion

This project demonstrates a basic container runtime with monitoring capabilities using user-space tools and kernel modules, showcasing OS-level resource management and isolation.

---

