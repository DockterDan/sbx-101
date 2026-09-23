<!--
layout: section
theme: dark
eyebrow: Live
-->

# Live demonstration

We will create a sandbox, inspect it, and change its network policy while it
runs. Follow along at **bit.ly/sbx101-cmd**.

---

<!-- layout: default -->

# A machine in seconds

```bash terminal-id=demo
sbx version
```

```bash terminal-id=demo
sbx create --name sandbox-alpha claude ~/workshop/sbx
```

::terminal{id=demo height=300}

---

<!-- layout: default -->

# Inspect and govern the running sandbox

```bash terminal-id=demo
sbx ls
```

```bash terminal-id=demo
sbx policy allow network nasa.gov
```

```bash terminal-id=demo
sbx policy ls
```

::terminal{id=demo height=300}

---

<!--
layout: title
byline: Docker Sandboxes
-->

# Run agents freely. Safely.

Everything from this session is yours to keep running.

- The full workshop, self-guided: **dockterdan.github.io/sbx-101**
- The follow-along terminals: **bit.ly/sbx101-cmd**
- The real CLI on your machine: **bit.ly/sbx101-setup**
