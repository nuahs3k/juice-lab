# 🛡️ Vulnerability Scanning Lab with Juice Shop

This project demonstrates a simple **vulnerability scanning workflow** using the OWASP Juice Shop (an intentionally vulnerable web app).  
It shows how to set up the lab with Docker, run scans using **Nmap** and **Nikto**, and collect results for analysis.

---

## 🔧 Tools Used
- **Docker Desktop** (to run containers)
- **OWASP Juice Shop** (vulnerable app)
- **Nmap** (network scanning & service detection)
- **Nikto** (web vulnerability scanner)
- **PowerShell** (for running commands & saving output)

---

## 📂 Project Layout
juice-lab/
│── scan-results/ # Output from scanners
│ ├── nmap_full_tcp.txt
│ ├── nmap_vuln_scan.txt
│ └── nikto_report.txt
│── README.md

yaml
Copy code

---

## 🚀 Step-by-Step Instructions

### 1. Install & Open Docker Desktop
- Download: [Docker Desktop](https://www.docker.com/products/docker-desktop/)  
- Make sure Docker is running before continuing.

---

### 2. Create a Working Folder
```powershell
mkdir $HOME\juice-lab\scan-results
cd $HOME\juice-lab
3. Create a Docker Network
This keeps the vulnerable app and scanners on the same network.

powershell
Copy code
docker network create vuln-net
4. Run OWASP Juice Shop
powershell
Copy code
docker run -d --name juice2 --network vuln-net bkimminich/juice-shop:latest
-d → detached mode (runs in background)

--name juice2 → container name (use juice2 if juice already exists)

--network vuln-net → attach to the same network as scanners

To confirm it’s running:

powershell
Copy code
docker ps
5. Run Nmap Full TCP Scan
Scans all ports and detects services.

powershell
Copy code
docker run --rm --network vuln-net instrumentisto/nmap:latest -p- -sS -sV -T4 juice2 > scan-results\nmap_full_tcp.txt
6. Run Nmap Vulnerability Scan
Uses vulnerability detection scripts.

powershell
Copy code
docker run --rm --network vuln-net instrumentisto/nmap:latest --script vuln juice2 > scan-results\nmap_vuln_scan.txt
7. Run Nikto Scan
Scans for web vulnerabilities.

powershell
Copy code
docker run --rm --network vuln-net sullo/nikto:latest -h http://juice2 > scan-results\nikto_report.txt
8. View Results
Check the scan outputs in the scan-results folder:

powershell
Copy code
Get-Content .\scan-results\nmap_full_tcp.txt -TotalCount 40
Get-Content .\scan-results\nmap_vuln_scan.txt -TotalCount 40
Get-Content .\scan-results\nikto_report.txt -TotalCount 40

⚠️ Troubleshooting
Docker Internal Server Error (500):
→ Start/restart Docker Desktop.

Container name conflict:

powershell
Copy code
docker rm -f juice
Then rerun with --name juice2.

Folder not found (scan-results):
→ Make sure you created it before saving outputs:

powershell
Copy code
mkdir $HOME\juice-lab\scan-results
🔮 Future Improvements
Automate scans with a single PowerShell script.

Add other scanners (OpenVAS, Nessus).

Deploy in a cloud lab environment.

ℹ️ Notes
Screenshots were originally created during testing but not included here to avoid exposing personal file paths.

All results can be reproduced by following the steps above.
