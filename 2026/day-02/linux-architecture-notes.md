# Linux Architecture, Processes, and systemd

## 1. Core Components of Linux
* **Kernel:** The core of the OS that directly manages hardware resources (CPU, RAM, Disks, Network). It acts as the bridge between software and hardware.
* **User Space:** The memory area where all user applications (e.g., Tomcat, JBoss, WebLogic, databases, shells) run. Applications here must request resources from the Kernel using **System Calls**.
* **systemd (PID 1):** The initialization daemon that starts immediately after the kernel boots. It acts as the parent of all user space processes, managing services, mount points, and system initialization in parallel.

---

## 2. Process Lifecycle & States
Every running program is a process identified by a unique **PID**. They transitions through the following states:
* **Running (R):** Currently executing on a CPU core or in the run queue.
* **Interruptible Sleep (S):** Waiting for an event or signal (e.g., waiting for database input). Wakes up easily.
* **Uninterruptible Sleep (D):** Waiting for a hardware resource/disk I/O. Cannot be interrupted.
* **Stopped (T):** Execution suspended manually (e.g., via `Ctrl + Z` or `SIGSTOP`).
* **Zombie (Z):** Process execution finished, but the parent process hasn't read its exit status yet. It remains in the system process table.

---

## 3. Why systemd Matters for DevOps
* **Parallelization:** Boosts boot speeds by starting services concurrently rather than sequentially.
* **Process Tracking (cgroups):** Grouping processes belonging to a service under a control group, ensuring that killing a service stops all its spawned subprocesses too.
* **Automated Recovery:** Can be configured to automatically restart critical application servers (like JBoss or Tomcat) if they crash.

---

## 4. Top 5 Daily Troubleshooting Commands
1. `ps auxf` – Displays all running processes in a hierarchical tree format to track parent-child relationships.
2. `top` – Shows real-time system performance, CPU/memory usage, and active tasks.
3. `systemctl status <service>` – Inspects the state and recent logs of a systemd unit.
4. `journalctl -u <service> -e --no-pager` – Fetches recent log entries directly from the systemd journal for the specified service.
5. `kill -15 <PID>` (SIGTERM) / `kill -9 <PID>` (SIGKILL) – Gracefully terminates or forcefully kills stuck application processes.
