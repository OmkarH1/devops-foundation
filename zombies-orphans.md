# Zombies and Orphans (Linux Processes)

This note explains **zombie** and **orphan** processes clearly.

---

## Zombie Process

### Definition

A **zombie** is a process that has **finished execution** but still has an entry in the **process table** because its **parent has not collected its exit status**.

### Why it happens

* Child calls `exit()`
* Kernel keeps PID + exit status
* Parent has **not called `wait()` / `waitpid()`**

### Characteristics

* No CPU usage
* No memory (only process table entry)
* State shown as **Z** or **<defunct>**

### How to see

```bash
ps -ef | grep Z
```

### How zombies are cleared

* Parent calls `wait()`
* Or parent dies → child is re-parented to PID 1, which reaps it

### Key point (interview)

> Zombies exist to allow the parent to read the child’s exit status.

---

## Orphan Process

### Definition

An **orphan** is a process whose **parent has terminated**, but the process itself is still running.

### What happens next

* Kernel re-parents the orphan to **PID 1 (init/systemd)**
* PID 1 becomes the new PPID

### Characteristics

* Fully running process
* Continues normal execution
* Safely managed by PID 1

### How to see

```bash
ps -o pid,ppid,cmd
```

### Key point (interview)

> Orphans are not a problem; they are automatically adopted by PID 1.

---

## Zombie vs Orphan (Comparison)

| Feature       | Zombie               | Orphan           |
| ------------- | -------------------- | ---------------- |
| Process state | Exited               | Running          |
| CPU / Memory  | No                   | Yes              |
| Parent alive  | Yes (not waiting)    | No               |
| PPID          | Original parent      | 1 (systemd/init) |
| Risk          | Yes (PID table fill) | No               |

---

## Interview One-Liners

* **Zombie:** A terminated process waiting for its parent to collect exit status.
* **Orphan:** A running process whose parent has died and is adopted by PID 1.

---

## Common Traps

* ❌ Zombies consume CPU
* ❌ Orphans are dangerous
* ❌ Zombies can run code

---

## Practical Tip

If you see many zombies:

* Fix the parent process
* Ensure it properly calls `wait()`

---

End of notes.
