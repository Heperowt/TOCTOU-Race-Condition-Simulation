# TOCTOU (Time-of-Check to Time-of-Use) Race Condition Simulation

A Python-based Proof of Concept (PoC) demonstrating a **Time-of-Check to Time-of-Use (TOCTOU)** vulnerability. This repository simulates a file system race condition where an attacker exploits the time gap between a security check and the actual use of a file by using symbolic links.

📖 **Detailed Read:** For a complete theoretical breakdown, step-by-step analysis, and mitigation strategies for this vulnerability, check out my article:

 **Read the Full Article Here 👉 https://medium.com/@mahdisaghayi/four-most-common-application-vulnerabilities-you-should-be-aware-of-0fb0a2c5bfba**

## The Vulnerability (Check & Use)
The vulnerability occurs when a program checks for file permissions (`os.access`) and then opens the file (`open`) as two separate, non-atomic operations. 

1. **TOC (Time-of-Check):** The victim program verifies that it has permission to write to a harmless file (`safe.txt`).
2. **The Gap:** A slight delay occurs during execution (simulated here using `time.sleep()`).
3. **The Attack:** The background attacker thread jumps in, deletes `safe.txt`, and replaces it with a **symlink** pointing to a critical system file (`sensitive.txt`).
4. **TOU (Time-of-Use):** The victim program wakes up and blindly writes its data to `safe.txt` (which is now a tunnel to the sensitive file), effectively overwriting critical data with authorized privileges.

## 🛠️ Prerequisites

**Important Note for Windows Users:** Creating symbolic links (`os.symlink`) on Windows requires elevated privileges. To successfully run this simulation on Windows, you must either:
* Run Visual Studio Code (or your terminal) as **Administrator**.
* Enable **Developer Mode** in Windows Settings (`Settings > System > For developers`).
