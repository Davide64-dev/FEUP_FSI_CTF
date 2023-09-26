# Trabalho realizado nas Semanas #2 e #3

## Identificação

- Citrix ADC enhances application performance with traffic optimization, security, and balancing across networks, vital for businesses.
- Critical CVE-2023-3519 vulnerability in Citrix ADC and Gateway exposes organizations to remote code execution and data compromise.
- This low-complexity threat requires authenticated users to insert malicious payloads into the SSL VPN portal, potentially risking widespread access.
- Organizations must urgently address this vulnerability to mitigate the risk of unauthorized access and data breaches.
## Catalogação

- item1
- item2
- item3
- item4

## Exploit: Remote Code Execution (RCE)

- In NetScaler ADC (formerly known as Citrix ADC) and NetScaler Gateway (formerly known as Citrix Gateway). 
- Uploading files containing malicious web shells and scripts to the vulnerable system, the attacker gains unauthorized access and control over it from a remote location. 
- This enables him to scan networks, execute arbitrary commands, and extract sensitive information from the compromised environment.
- [Metasploit](https://www.rapid7.com/db/modules/exploit/freebsd/http/citrix_formssso_target_rce/)

## Ataques

- [duo.com](https://duo.com/decipher/attacks-against-citrix-cve-2023-3519-bug-escalate) reports attackers exploiting PHP webshell vulnerability for persistent access to hosts, potentially compromising security in the future.
- A U.S. critical infrastructure organization faced a remote code execution attack, allowing hackers to plant a web shell and steal active directory data, as reported by [bleepingcomputer](https://www.bleepingcomputer.com/news/security/netscaler-adc-bug-exploited-to-breach-us-critical-infrastructure-org/).

