> Join this session: **bit.ly/sbx101-cmd**

### Setup

*Terminal A*

```bash terminal-id=a
sbx version
```

```bash terminal-id=a
sbx login
```

```bash terminal-id=a
sbx policy reset
```

```bash terminal-id=a
2
```

```bash terminal-id=a
sbx secret set -g anthropic
```

```text no-run-button
sk-ant-api03-sbx101-demo-2f9d4e8a7c1b0356
```

```bash terminal-id=a
sbx secret ls
```

----

### Step 1 · Your first sandbox

*Terminal A*

```bash terminal-id=a
sbx create --name sandbox-alpha claude ~/workshop/sbx
```

```bash terminal-id=a
sbx ls
```

```bash terminal-id=a
sbx run --name sandbox-alpha
```

```prompt terminal-id=a
Run uname -a; whoami; pwd and tell me what you see.
```

```bash terminal-id=a
sbx create --name sandbox-bravo claude ~/workshop/sbx
```

```bash terminal-id=a
sbx ls
```

```bash terminal-id=a
sbx run --name sandbox-bravo
```

```bash terminal-id=a
sbx stop sandbox-bravo
```

```bash terminal-id=a
sbx rm sandbox-bravo
```

```bash terminal-id=a
y
```

```bash terminal-id=a
sbx ls
```

----

### Step 2 · Isolation proof

*Terminal A*

```bash terminal-id=a
sbx run --name sandbox-alpha
```

*Terminal B*

```bash terminal-id=b
echo "Written from the host" > ~/workshop/sbx/host-to-agent.txt
```

*Terminal A*

```prompt terminal-id=a
There should be a file called host-to-agent.txt in your workspace. Run cat host-to-agent.txt and tell me what you see.
```

```prompt terminal-id=a
Create a file called agent-to-host.txt in your workspace with the text "Written from the agent".
```

*Terminal B*

```bash terminal-id=b
ls -la ~/workshop/sbx
```

```bash terminal-id=b
cat ~/workshop/sbx/agent-to-host.txt
```

```bash terminal-id=b
echo "This file lives in your home, not the workspace" > ~/workshop/host-marker.txt
```

*Terminal A*

```prompt terminal-id=a
Try to read a file called host-marker.txt. Run cat ~/workshop/host-marker.txt 2>&1 and also try the absolute path /Users/me/workshop/host-marker.txt, and tell me what happens.
```

*Terminal B*

```bash terminal-id=b
sbx secret ls
```

*Terminal A*

```prompt terminal-id=a
Run echo "ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY" and env | grep -i anthropic and tell me what you see.
```

*Terminal B*

```bash terminal-id=b
rm ~/workshop/host-marker.txt ~/workshop/sbx/host-to-agent.txt ~/workshop/sbx/agent-to-host.txt
```

----

### Step 3 · Network policy

*Terminal B*

```bash terminal-id=b
sbx policy ls
```

*Terminal A*

```prompt terminal-id=a
Run these five curls and tell me what you see for each: curl -s https://github.com -o /dev/null -w "github.com: %{http_code} (%{size_download} bytes)\n" ; then the same for pypi.org, reddit.com, wikipedia.org, and nasa.gov.
```

*Terminal B*

```bash terminal-id=b
sbx policy log sandbox-alpha
```

```bash terminal-id=b
sbx policy allow network nasa.gov
```

```bash terminal-id=b
sbx policy deny network pypi.org
```

```bash terminal-id=b
sbx policy ls
```

```bash terminal-id=b
sbx policy ls --wide
```

*Terminal A*

```prompt terminal-id=a
Run curl -s https://pypi.org -o /dev/null -w "pypi.org: %{http_code} (%{size_download} bytes)\n" and then run pip install requests 2>&1 | tail -3. Tell me what you see for each.
```

*Terminal B*

```bash terminal-id=b
sbx policy deny network example.com
```

```bash terminal-id=b
sbx policy ls
```

```bash terminal-id=b
sbx policy rm network --resource example.com
```

```bash terminal-id=b
sbx policy ls
```

----

### Step 4 · Tools via MCP

*Terminal B*

```bash terminal-id=b
sbx mcp --help
```

```bash terminal-id=b
export SBX_MCP_URL=https://registry.modelcontextprotocol.io
```

```bash terminal-id=b
sbx daemon stop
```

```bash terminal-id=b
sbx daemon start -d
```

```bash terminal-id=b
sbx mcp --help
```

```bash terminal-id=b
sbx mcp add wiki-mcp --command docker --args "run,-i,--rm,mcp/wikipedia-mcp"
```

```bash terminal-id=b
sbx mcp inspect wiki-mcp
```

```bash terminal-id=b
sbx mcp add duck-mcp --command docker --args "run,-i,--rm,mcp/duckduckgo"
```

```bash terminal-id=b
sbx mcp inspect duck-mcp
```

```bash terminal-id=b
sbx mcp ls
```

*Terminal A*

```bash terminal-id=a
sbx run --name sandbox-alpha
```

```prompt terminal-id=a
Fetch https://en.wikipedia.org/api/rest_v1/page/summary/Artemis_program and tell me the HTTP status and any body you got back.
```

*Terminal B*

```bash terminal-id=b
sbx policy log sandbox-alpha
```

```bash terminal-id=b
sbx mcp load wiki-mcp --sandbox sandbox-alpha
```

*Terminal A*

```bash terminal-id=a
sbx stop sandbox-alpha
```

```bash terminal-id=a
sbx rm sandbox-alpha
```

```bash terminal-id=a
y
```

```bash terminal-id=a
sbx create --name sandbox-alpha --static-mcp wiki-mcp --static-mcp duck-mcp claude ~/workshop/sbx
```

```bash terminal-id=a
sbx run --name sandbox-alpha
```

```prompt terminal-id=a
Using your MCP tools — wiki-mcp for background and duck-mcp for recent news — tell me what's happening with NASA's Artemis program. At the end, tell me which tools you used, and if anything didn't work, explain what happened.
```

*Terminal B*

```bash terminal-id=b
sbx policy log sandbox-alpha
```

----

### Step 5 · Hardened output

*Terminal B*

```bash terminal-id=b
sbx policy rm network --resource pypi.org
```

```bash terminal-id=b
sbx policy ls
```

```bash terminal-id=b
mkdir -p ~/workshop/sbx/research-app
```

*Terminal A*

```bash terminal-id=a
sbx run --name sandbox-alpha
```

```prompt terminal-id=a
There's a research-app/ directory in the workspace with a Dockerfile already in it. Build me a small, polished dashboard that mixes two threads: open-source projects from GitHub and current space missions, connected in an unexpected way. Research with real sources — try nasa.gov, space news outlets, and GitHub — cap yourself at 5-6 external requests and move on when something is blocked. Constraints: only write app.py; stdlib only; embed all research data inline; serve HTTP on port 8000 on 0.0.0.0; light clean design, no emoji; include a Sources section. Sanity-check it with curl before reporting back.
```

*Terminal B*

```bash terminal-id=b
sbx policy log sandbox-alpha
```

```bash terminal-id=b
cat sbx/research-app/app.py
```

```bash terminal-id=b
cd ~/workshop/sbx/research-app
```

```bash terminal-id=b
docker build -t research-app:v1 .
```

```bash terminal-id=b
docker run -d --rm -p 8000:8000 --name research-app research-app:v1
```

```bash terminal-id=b
curl -s http://localhost:8000 | head -8
```

```bash terminal-id=b
docker stop research-app
```

```bash terminal-id=b
docker scout quickview research-app:v1
```

```bash terminal-id=b
docker scout cves research-app:v1
```

```bash terminal-id=b
docker login dhi.io
```

```bash terminal-id=b
sed -i.bak 's|FROM python:3.14-slim|FROM dhi.io/python:3.14|' Dockerfile
```

```bash terminal-id=b
docker build -t research-app:v2 .
```

```bash terminal-id=b
docker run -d --rm -p 8000:8000 --name research-app research-app:v2
```

```bash terminal-id=b
docker stop research-app
```

```bash terminal-id=b
docker scout quickview research-app:v2
```

```bash terminal-id=b
docker scout compare --to research-app:v1 research-app:v2
```

----

### Step 6 · Kits

*Terminal B*

```bash terminal-id=b
mkdir -p ~/workshop/kits/docker-review/files/workspace/.claude/skills/docker-review
```

```bash terminal-id=b
sbx kit validate ~/workshop/kits/docker-review/
```

```bash terminal-id=b
sbx create --name sandbox-kits --kit ~/workshop/kits/docker-review/ claude ~/workshop/sbx
```

```bash terminal-id=b
ls -la ~/workshop/sbx/.claude/skills/docker-review/
```

*Terminal A*

```bash terminal-id=a
sbx run --name sandbox-kits
```

```prompt terminal-id=a
Review the Dockerfile in research-app/ against best practices and give me a structured assessment.
```

```bash terminal-id=a
sbx stop sandbox-kits
```

```bash terminal-id=a
sbx rm sandbox-kits
```

```bash terminal-id=a
y
```
