# systemd Unit Types 

This note explains **systemd unit types**, what they are used for, and how to identify them in real systems.

---

## What is a systemd Unit?

A **unit** is a resource that systemd knows how to manage.
Examples:

* A service (nginx)
* A mount point (/data)
* A timer (cron replacement)
* A device

Unit files are usually stored in:

* `/usr/lib/systemd/system/`
* `/etc/systemd/system/`

---

## Common systemd Unit Types

### 1️⃣ service (.service)

Used to manage **services / daemons**.

Examples:

* nginx.service
* sshd.service
* docker.service

Common service types:

* `simple`
* `forking`
* `oneshot`
* `notify`

Example:

```ini
[Service]
ExecStart=/usr/sbin/nginx
```

Interview point:

> Most day-to-day systemd work involves `.service` units.

---

### 2️⃣ target (.target)

Used for **grouping units** and defining system state.

Examples:

* multi-user.target
* graphical.target
* network.target

Analogy:

> Targets are like **runlevels** in SysV init.

Example:

```bash
systemctl get-default
```

---

### 3️⃣ timer (.timer)

Used for **time-based execution** (cron alternative).

Examples:

* logrotate.timer
* apt-daily.timer

Pairs with a `.service` file.

Example:

```bash
systemctl list-timers
```

---

### 4️⃣ socket (.socket)

Used for **socket-based activation**.

systemd listens on a socket and starts the service only when traffic arrives.

Examples:

* sshd.socket
* docker.socket

Benefit:

* Faster boot
* On-demand services

---

### 5️⃣ mount (.mount)

Used to manage **filesystem mount points**.

Example:

* data.mount → mounts `/data`

Example:

```bash
systemctl status data.mount
```

---

### 6️⃣ automount (.automount)

Automatically mounts filesystems **when accessed**.

Used with `.mount` units.

Example:

* home.automount

---

### 7️⃣ device (.device)

Represents **kernel-recognized devices**.

Example:

* dev-sda1.device

Automatically created by systemd.

---

### 8️⃣ path (.path)

Monitors filesystem paths and triggers services on changes.

Example use-case:

* Trigger a service when a file appears

Example:

```bash
systemctl list-units --type=path
```

---

### 9️⃣ snapshot (.snapshot)

Captures the **current system state**.

Used to rollback temporarily.

Example:

```bash
systemctl snapshot test.snap
```

---

### 🔟 swap (.swap)

Manages **swap devices or files**.

Example:

* swapfile.swap

---

## Quick Comparison Table

| Unit Type | Purpose                 |
| --------- | ----------------------- |
| service   | Manage services         |
| target    | Group units             |
| timer     | Scheduled execution     |
| socket    | On-demand service start |
| mount     | Filesystem mount        |
| automount | Lazy mount              |
| device    | Hardware devices        |
| path      | File change trigger     |
| snapshot  | System state save       |
| swap      | Swap management         |


## Most Important for DevOps

Focus on:

* `.service`
* `.target`
* `.timer`
* `.socket`

---

End of notes.
