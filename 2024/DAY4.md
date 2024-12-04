# I'm all atomic inside!

## The story
SOC-mas is approaching! And the town of Warewille started preparations for the grand event.

Glitch, a quiet, talented security SOC-mas engineer, had a hunch that these year's celebrations would be different. With looming threats, he decided to revamp the town's security defences. Glitch began to fortify the town's security defences quietly and meticulously. He started by implementing a protective firewall, patching vulnerabilities, and accessing endpoints to patch for security vulnerabilities. As he worked tirelessly, he left "breadcrumbs," small traces of his activity.

Unaware of Glitch's good intentions, the SOC team spotted anomalies: Logs showing admin access, escalation of privileges, patched systems behaving differently, and security tools triggering alerts. The SOC team misinterpreted the system modifications as a sign of an insider threat or rogue attacker and decided to launch an investigation using the Atomic Red Team framework.

## Learning Objectives
- Learn how to identify malicious techniques using the MITRE ATT&CK framework.
- Learn about how to use Atomic Red Team tests to conduct attack simulations.
- Understand how to create alerting and detection rules from the attack tests.

## Topics
- **Detection Gaps**: Understand gaps in security monitoring and their impact on incident response.  
- **Cyber Attacks and the Kill Chain**: Explore the stages of a cyber attack to better predict and defend against threats.  
- **MITRE ATT&CK**: Learn how to map attacker behaviors using this comprehensive framework.  
- **Atomic Red Team Library**: Discover a set of open-source tests for simulating real-world attacks.

- **Dropping the Atomic**: Understand the process of preparing and initiating an atomic test.  
- **Running an Atomic**: Execute attack simulations to assess system defenses.  
- **Detecting the Atomic**: Analyze system behavior and logs to identify indicators of compromise.  
- **Alerting on the Atomic**: Set up alerts and improve monitoring based on test results.

## Challenge
Glitch plans an attack simulation mimicking a ransomware attack to test Wareville's defenses.  
He seeks help identifying the correct atomic test using a command and scripting interpreter, running the test, and extracting artifacts to craft detection rules.

## Questions and Answers
### What was the flag found in the `.txt` file associated with the `PhishingAttachment.xslm` artifact?
1. Open PowerShell as Administrator.  
2. Run:  

``` Powershell
Invoke-AtomicTet T1566.001 -TestNumbers 1 -ShowDetails
```
3. Refresh the Event Viewer and analyze the chronological logs for relevant events:  
- PowerShell creates a process to download `PhishingAttachment.xlsm`.  
- A file `PhishingAttachment.xlsm` is created.  
4. Navigate to `C:\Users\Administrator\AppData\Local\Temp\` and open `PhishingAttachment.txt`.

- **Flag**: `THM{GlitchTestingForSpearphishing}`

Cleanup command: Invoke-AtomicTest T1566.001-1 -cleanup.

---

### What ATT&CK technique ID would be our point of interest?
Search: *command and scripting interpreter technique* (https://attack.mitre.org/techniques/T1059/)
- **ID**: T1059

---

### What ATT&CK Subtechnique ID focuses on the Windows Command Shell?
Search: *Windows Command Shell subtechnique*.  
- **ID**: T1059.003

---

### What is the name of the Atomic Test to be simulated?
1. Open PowerShell as Administrator.  
2. Run:
``` Powershell
Invoke-AtomicTet T1059.003 -ShowDetails
```
- **Name**: Simulate BlackByte Ransomware Print Bombing

---

### What is the name of the file used in the test?
- **File**: `Wareville_Ransomware.txt`

---

### What is the flag found from this Atomic Test?
1. Analyze the following command:  
cmd /c "for /l %x in (1,1,1) do start wordpad.exe /p 
C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt"

2. Navigate to: `C:\Tools\AtomicRedTeam\atomics\T1059.003\src\Wareville_Ransomware.txt`
- **Flag**: Found in `Wareville_Ransomware.txt`


## Conclusion
Through this challenge, we explored the importance of simulating cyberattacks to strengthen security defenses and enhance detection capabilities.  

By leveraging the MITRE ATT&CK framework and Atomic Red Team tests, we demonstrated how to simulate real-world threats, extract valuable artifacts, and craft effective detection rules. 
These steps help identify gaps in security monitoring, improve incident response, and better prepare against sophisticated attacks like ransomware and spearphishing.  
