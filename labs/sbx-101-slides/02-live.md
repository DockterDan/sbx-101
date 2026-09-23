<!--
layout: section
theme: dark
eyebrow: Live
-->

# Live demonstration

We will create a sandbox, inspect it, and change its network policy while it
runs.

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

<!-- layout: default -->

# Every connection is on the record

```bash terminal-id=demo
sbx policy log sandbox-alpha
```

::terminal{id=demo height=300}

---

<!-- layout: default -->

# Step inside the machine

```bash terminal-id=demo
sbx run --name sandbox-alpha
```

```prompt terminal-id=demo
Run uname -a; whoami; pwd and tell me what you see.
```

```bash terminal-id=demo
/exit
```

::terminal{id=demo height=300}

---

<!--
layout: title
byline: Docker Sandboxes
-->

# Run agents freely. Safely.

Agents are most useful when you can say yes to them — and you can only say
yes when you know exactly what they can see, reach, and ship.

- The security boundary comes from infrastructure, not from the prompt.
- A sandbox is the agent's own machine: one directory, a governed network, and secrets it can use but never read.
- It is available today, free, for any developer.
