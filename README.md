# Lab Sheet — Deploying an Application on a DigitalOcean Droplet with Docker (React Example)

**Version:** 2025-09-18  
**Goal:** Deploy a React application inside Docker on a DigitalOcean Droplet, verify the container’s file system, and manage it with Docker Compose.

---

## Submission Requirements (What to Hand In)
- **Screenshots** listed under each step (exactly as requested).
- **Reflection (2–3 lines per step):** Describe **what** you did, **why** it’s necessary, and the **benefit**.
- Submit as a single **PDF** or a **zipped** folder of images + a **Markdown/Doc** file with your reflections.

> Replace placeholders like `<droplet_ip>`, `<github_repo_url>`, and `<container-id>` with real values.

---

## Prerequisites (Read First)
- You already have a working Droplet (e.g., **Ubuntu 22.04 LTS**) with a **non-root** sudo user and **SSH keys** set up.
- **Docker Engine** and **Docker Compose v2** are installed (Steps A & B below show how).  
- Your firewall (e.g., **UFW**) allows the chosen HTTP port (e.g., **80** or **8080**).

### A) Install Docker Engine
```bash
# As a sudo-enabled user on Ubuntu 22.04+
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Let your user run docker without sudo (log out/in to take effect)
sudo usermod -aG docker $USER
```

### B) Verify Docker & Compose
```bash
docker --version
docker compose version
docker run --rm hello-world
```

**Screenshot to Capture:**  
- `docker --version` and `docker compose version` outputs.  
- `hello-world` success message.

**Reflection (2–3 lines):**  
- What did you install and verify?  
- Why do we need Docker & Compose?  
- Benefit for reproducible deployments?

---

## 1) Clone the Lab1 React Project to Your Droplet
If you have a GitHub repo:
```bash
sudo apt-get update && sudo apt-get install -y git
cd ~
git clone <github_repo_url> lab1-react
cd lab1-react
ls -la
```
If you are uploading from local instead, you can `scp` the folder to the Droplet:
```bash
# On your local machine (example)
scp -r ./lab1-react <user>@<droplet_ip>:~/
```

**Screenshot to Capture:**  
- Terminal showing `git clone` (or `scp`) success and `ls -la` inside the project folder.

**Reflection (2–3 lines):**  
- What did you clone/copy?  
- Why keep source in a dedicated directory?  
- Benefit for organization and CI/CD?

---

## 2) Create a `Dockerfile` at the Project Root
Create a **multi-stage** Dockerfile to build React and serve with **Nginx**:
```dockerfile
# ---- Build stage ----
FROM node:18-alpine AS builder
WORKDIR /app
# If you have a lockfile, copy it first for better caching
COPY package*.json ./
RUN npm ci --no-audit --no-fund
COPY . .
# Build production assets (uses Vite or CRA scripts depending on your project)
# For Vite: npm run build
# For CRA:  npm run build
RUN npm run build

# ---- Runtime stage ----
FROM nginx:alpine
# Copy build to Nginx html directory (adjust if your build directory differs)
COPY --from=builder /app/dist /usr/share/nginx/html
# If using CRA the build folder is /app/build instead of /app/dist
# COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
# Minimal healthcheck (optional)
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -qO- http://localhost/ || exit 1
```

> **Note:** If your React toolchain outputs to `build` instead of `dist`, switch the `COPY` path accordingly.

**Screenshot to Capture:**  
- A terminal view (`cat Dockerfile`) showing your final Dockerfile contents.

**Reflection (2–3 lines):**  
- What does multi-stage build achieve?  
- Why use Nginx for static files?  
- Benefit for small, secure runtime images?

---

## 3) Build the Image and Run the Container
```bash
# From the project root where Dockerfile exists
docker build -t lab1-react:v1 .

# Run the container (choose a port to expose)
# Option A: map to port 80 (requires the port to be open and free)
docker run -d --name lab1-web -p 80:80 lab1-react:v1

# Option B: map to port 8080 (use this if 80 is busy or restricted)
# docker run -d --name lab1-web -p 8080:80 lab1-react:v1

# Verify it’s running
docker ps
# Test locally from the Droplet
curl -I http://localhost        # or http://localhost:8080
```

**Screenshot to Capture:**  
- `docker build` ending lines (success message).  
- `docker ps` showing the `lab1-web` container and ports.  
- `curl -I http://localhost` (or `:8080`) returning `200 OK`.

**Reflection (2–3 lines):**  
- What did build & run do?  
- Why expose ports?  
- Benefit for confirming app availability?

---

## 4) Inspect Files Inside the Running Container
> Nginx images typically **don’t include `bash`**. Use `sh` if `bash` is missing.
```bash
# Find the container ID or name from `docker ps`
docker exec -it <container-id-or-name> sh
# Inside the container:
ls -la /usr/share/nginx/html
exit
```

**Screenshot to Capture:**  
- The `ls -la /usr/share/nginx/html` output **inside** the container.

**Reflection (2–3 lines):**  
- What did you verify inside the container?  
- Why inspect filesystem content?  
- Benefit for debugging image/runtime?

---

## 5) Create a `docker-compose.yaml` at Project Root
This Compose file builds the image and runs the Nginx container. It also shows an optional **dev** service for live development (commented).
```yaml
# docker-compose.yaml
services:
  web:
    build:
      context: .
    image: lab1-react:compose
    container_name: lab1-web
    ports:
      - "80:80"         # change to "8080:80" if needed
    restart: unless-stopped

  # --- Optional: development service (uncomment if you want hot reload) ---
  # dev:
  #   image: node:18-alpine
  #   working_dir: /app
  #   volumes:
  #     - ./:/app
  #   command: sh -c "npm ci && npm run dev -- --host"
  #   ports:
  #     - "5173:5173"   # Vite default; adjust for CRA dev server
  #   environment:
  #     - NODE_ENV=development
  #   # Useful if you need extra packages for native deps
  #   # extra_hosts:
  #   #   - "host.docker.internal:host-gateway"
```

**Screenshot to Capture:**  
- Terminal view (`cat docker-compose.yaml`) showing the file contents.

**Reflection (2–3 lines):**  
- What does Compose simplify?  
- Why define services declaratively?  
- Benefit for portability and team workflows?

---

## 6) Run Compose, Check Logs and Resource Usage
```bash
# Start in detached mode
docker compose up -d

# Verify containers
docker compose ps

# Follow logs
docker compose logs -f

# Open another terminal to check container resource usage
docker stats

# When done, stop and clean up (optional)
# docker compose down
```

**Screenshot to Capture:**  
- `docker compose ps` showing the running service(s).  
- A snippet of `docker compose logs -f` output.  
- `docker stats` for the running container(s).

**Reflection (2–3 lines):**  
- What did Compose run/manage?  
- Why inspect logs and stats?  
- Benefit for monitoring and troubleshooting?

---

## (Optional) Health Check & Nginx Index Test
If you want to confirm the Nginx root is served correctly:
```bash
# On the host, fetch HTML and view the first line
curl -s http://localhost | head -n 3
```

**Screenshot to Capture:**  
- The output from the `curl` command showing your app’s HTML.

**Reflection (2–3 lines):**  
- What does this confirm?  
- Why test HTTP responses?  
- Benefit for end-to-end verification?

---

## Final Self‑Check
- [ ] Docker & Compose installed and verified.
- [ ] Project cloned to Droplet and accessible.
- [ ] Image builds successfully from `Dockerfile`.
- [ ] Container responds on the mapped port (80 or 8080).
- [ ] You can `exec` into the container and see built assets.
- [ ] Compose file works (`docker compose up -d`), logs and stats checked.

---

## Tips for Clean Submissions
- Blur/redact secrets, private IPs, or tokens in screenshots.
- Use readable terminal font sizes and high contrast.
- If you changed default ports, mention them in your reflection file.
- Export images to a single PDF or zip with reflections and submit.

**End of Lab.**
