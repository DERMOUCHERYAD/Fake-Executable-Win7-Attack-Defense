# Fake Executable – Attack & Defense (Windows 7 / Kali)

## 📌 Overview
This repository contains a lab (TP) exploring human-targeted attacks using crafted executable files and the defensive lessons learned. The exercises were performed on **Windows 7 (target)** and **Kali Linux (attacker)** using the Metasploit framework and related tooling.

**Key topics covered:**
- Social engineering and human exploitation via malicious executables
- Creating payloads with `msfvenom`
- Bind TCP vs Reverse TCP payloads
- Bypassing firewalls using reverse HTTPS/tunneling
- Simple defenses: firewall rules, antivirus, and user awareness

> ⚠️ **Educational purpose only.** Do not use these techniques for illegal or unethical actions.

---

## 🛠 Tools used
- **Windows 7** (target machine)
- **Kali Linux** (attacker machine)
- **Metasploit Framework**
  - `msfvenom` — payload generation
  - `multi/handler` — session handling
- **Python** simple HTTP server (file hosting)
- `netstat`, `iptables`, and standard system utilities

---

## 📁 Repository contents
- `Fake-Executable-Win7-Attack_defense.docx` — full lab report (original)
- `examples/` — example commands and scripts (msfvenom/multi/handler snippets)
- `notes/` — short defense recommendations and checklist
- `README.md` — this file

---

## 🔍 Lab scenarios
1. **Bind TCP payload**: executable opens a listening port on the target (attacker connects to target).  
2. **Reverse TCP payload**: target initiates outbound connection to attacker (useful to bypass inbound filtering).  
3. **Reverse HTTPS payload**: encapsulate the reverse connection over HTTPS (ports 80/443) to evade strict egress filtering.  
4. **Port scanning / port fuzzing** to find allowed outbound ports when some ranges are blocked.  
5. **Firewall rule testing**: create/modify Windows Firewall rules and observe which payloads succeed.

---

## ⚠️ Notes on detection & defense
- **Antivirus/EPP** can detect many raw payloads — packers/obfuscation will often trigger alerts.  
- **Windows Firewall**: inbound filtering can block Bind TCP; proper outbound filtering prevents many Reverse payloads.  
- **Egress filtering & proxy**: forcing all outbound traffic through a proxy on ports 80/443 reduces ability to use arbitrary outbound ports.  
- **User awareness**: the most effective mitigation against malicious executables is training and phishing-resistant processes.  
- **Monitoring**: use host-based and network-based telemetry (process monitoring, netstat, IDS) to detect odd listening ports or unexpected outbound sessions.

---

## ⚙️ Example commands (high-level)
> *Only for lab / educational environments you own or are explicitly authorized to test.*

**Generate a bind TCP EXE**
```bash
msfvenom -p windows/shell_bind_tcp LHOST=0.0.0.0 LPORT=4444 -f exe -o bind_shell.exe

