Juice Shop Vulnerability Lab

This project sets up and scans OWASP Juice Shop
, an intentionally vulnerable web application used for practicing web security testing.
The lab demonstrates basic recon, vulnerability scanning, and reporting workflows with Dockerized tools.

📌 Project Overview

Goal: Practice running a vulnerable web app locally and scanning it with tools like Nmap and Nikto.

What I Did:

Deployed OWASP Juice Shop in Docker.

Created a dedicated Docker network to isolate vulnerable apps.

Ran network scans (nmap, nikto) against the container.

Collected outputs into organized files.

Documented issues/errors I encountered and how I solved them.

Pushed the project with results and screenshots to GitHub.

⚙️ Tech Stack

Platform: Windows 11 (PowerShell)

Containerization: Docker Desktop

Targets:

bkimminich/juice-shop (latest)

Tools Used:

instrumentisto/nmap
 (TCP scans, service version detection)

sullo/nikto
 (web server vulnerability scanning)

Version Control: Git + GitHub

🛠️ How to Run

Clone this repo:

git clone https://github.com/<your-username>/juice-lab.git
cd juice-lab


Create the Docker network:

docker network create vuln-net


Run Juice Shop:

docker run -d --name juice --network vuln-net bkimminich/juice-shop:latest


(If you get a container name conflict, use another name like juice2)

Run Nmap full TCP scan:

docker run --rm --network vuln-net instrumentisto/nmap:latest -p- -sS -sV -T4 juice > .\scan-results\nmap_full_tcp.txt


Run vulnerability scan:

docker run --rm --network vuln-net instrumentisto/nmap:latest --script vuln juice > .\scan-results\nmap_vuln_scan.txt


Run Nikto scan:

docker run --rm --network vuln-net sullo/nikto:latest -h http://juice:3000 > .\scan-results\nikto_report.txt


View results (top 40–60 lines):

Get-Content .\scan-results\nmap_vuln_scan.txt -TotalCount 60
Get-Content .\scan-results\nikto_report.txt -TotalCount 60

📂 Project Layout
juice-lab/
│
├── scan-results/              # All scanner outputs
│   ├── nmap_full_tcp.txt
│   ├── nmap_vuln_scan.txt
│   └── nikto_report.txt
│
├── screenshots/               # Manual screenshots during testing
│   ├── docker_network.png
│   ├── nmap_results.png
│   └── github_push.png
│
├── .gitignore                 # Excluded files
└── README.md                  # Documentation

🐛 Errors Encountered & Fixes
Error	Cause	Fix
Conflict. The container name "/juice" is already in use	Tried to reuse a container name	Removed old container (docker rm -f juice) or renamed new one (juice2)
Out-File: Could not find a part of the path 'C:\Users\...scan-results\nmap_full_tcp.txt'	Folder didn’t exist	Created scan-results directory before running scan
Unexpected token \n=== in PowerShell	Misused backticks in inline PowerShell	Broke command into separate lines or used semicolons properly
Git push rejected (remote not found / auth failed)	No GitHub remote or wrong credentials	Added remote with git remote add origin ... and used PAT for authentication
🔧 Troubleshooting

Docker not starting: Restart Docker Desktop.

Can’t access Juice Shop in browser: Use http://localhost:3000.

Nmap scans fail: Ensure container is on the same Docker network (--network vuln-net).

Git errors: Run git status to check for staged files. Use git pull --rebase before pushing.

🚀 Potential Updates

Add Metasploitable2 or DVWA containers to expand the lab.

Automate scanning + reporting with a PowerShell or Python script.

Build a Markdown → PDF pipeline for cleaner reports.

Expand screenshots into a proper walkthrough guide.

Add CI/CD (GitHub Actions) to auto-run scans on container startup.

✅ Status

Project completed successfully:

Vulnerable app deployed

Scans captured

Errors resolved

Repo structured and pushed to GitHub
