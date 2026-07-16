# SOC-Analyst-Invite-Only-Scenario
As an SOC Analyst, I assisted an L3 analyst by triaging and investigating two high-priority indicators of compromise (IoCs) flagged during morning operations.

As a Tier 1 SOC Analyst, I triaged and investigated two high-priority Indicators of Compromise (IoCs) escalated by our Tier 3 team: a suspicious SHA-256 file hash and a malicious IP address.
By leveraging threat intelligence platforms, I uncovered a multi-stage "ClickFix" phishing campaign targeting Discord users. This investigation allowed me to map the threat actor's entire attack chain—from the initial social engineering lure to the deployment of AsyncRAT and the final extraction of Google Chrome session cookies.
Below is the step-by-step breakdown of how I analyzed the artifacts, verified the threat, and documented the campaign.

### Tools Used
* **Threat Intelligence:** [VirusTotal](https://www.virustotal.com/gui/home/upload)

### SOC Analyst Scenario - Threat Information

<img width="1455" height="684" alt="1" src="https://github.com/user-attachments/assets/ec2c66d4-378d-4f9c-95f6-003ade9a1089" />
In this initial phase of the challenge, my objective was to establish the foundation for the threat investigation. As an SOC analyst supporting the incident response team at TrySecureMe, I was provided with two key indicators of compromise (IoCs) flagged early in the morning by a Tier 1 analyst:

Target IP Address: 101[.]99[.]76[.]120

Target SHA-256 Hash: 5d0509f68a9b7c415a726be75a078180e3f02e59866f193b0a99eee8e39c874f

These two specific data points served as the baseline inputs for my entire investigation. Armed with this initial information, my task was to spin up the lab environment and utilize OSINT resources, specifically VirusTotal, to research the reputation, behavior, and campaign context of these artifacts.

### SOC THreat Challenge Investigations and Results
<img width="1455" height="684" alt="2" src="https://github.com/user-attachments/assets/02aac74e-b926-4c7d-903f-3322b52b2724" />

### To begin the technical investigation, I utilized VirusTotal to analyze the flagged SHA-256 hash: 5d0509f68a9b7c415a726be75a078180e3f02e59866f193b0a99eee8e39c874f

<img width="1455" height="684" alt="3" src="https://github.com/user-attachments/assets/e2dbbd58-78a0-4172-81be-b3398e351892" />
<img width="1455" height="684" alt="4" src="https://github.com/user-attachments/assets/5aec5abd-ef85-4110-8a8e-5e56f6b86e39" />
<img width="1455" height="684" alt="5" src="https://github.com/user-attachments/assets/ce0083e3-f43e-425f-8698-391157e9fbda" />
<img width="1455" height="684" alt="6" src="https://github.com/user-attachments/assets/09113c66-0833-4eda-9167-0f2308af3492" />
<img width="1455" height="684" alt="7" src="https://github.com/user-attachments/assets/fce293c8-198f-48c8-9179-ffea702ea9d2" />
<img width="1455" height="684" alt="8" src="https://github.com/user-attachments/assets/3b5ee89a-cca7-4fcb-a052-3bcc45f63ef6" />
<img width="1455" height="684" alt="9" src="https://github.com/user-attachments/assets/553efde2-c4dc-4664-8524-9b37c0479328" />

### File Name Identification: Under the detection and basic properties, the malicious file was identified as syshelpers.exe.

### To answer the second question, I focused on identifying the exact file type of the malicious payload.

<img width="1455" height="684" alt="9" src="https://github.com/user-attachments/assets/fa91507d-18fd-4361-adca-158a8b2d7341" />
<img width="1455" height="684" alt="10" src="https://github.com/user-attachments/assets/b57f16b3-020b-4de5-b245-6c5b5bc9c5ba" />
<img width="1455" height="684" alt="11" src="https://github.com/user-attachments/assets/29c99b0d-c065-4dd2-a3c8-37c9ca62336b" />

### File Type: Confirmed it as a Win32 EXE

### To determine the origin and execution flow of the malicious payload, I investigated how the syshelpers.exe process was launched.

<img width="1455" height="684" alt="11" src="https://github.com/user-attachments/assets/ed84f8d3-85ed-4171-9224-019fbad1d8d5" />

### Inside VirusTotal, I navigated to the Relations tab for the target hash

<img width="1455" height="684" alt="12" src="https://github.com/user-attachments/assets/e23ba5dc-ead8-4b41-93df-ce2c6e33186c" />
<img width="1455" height="684" alt="13" src="https://github.com/user-attachments/assets/a6ca5628-189e-42f7-b190-46134dc301da" />

### Identifying the Parents: Scrolling down to the Execution Parents section, I found two files responsible for executing the payload: A Powershell script named 361GJX7J and A Win32 EXE binary named installer.exe

### Next, I focused on identifying what secondary payloads or configuration files were dropped onto the target host when syshelpers.exe executed.

<img width="1455" height="684" alt="14" src="https://github.com/user-attachments/assets/ba5af60f-9249-4e51-aaef-f4d6226581fe" />
<img width="1455" height="684" alt="15" src="https://github.com/user-attachments/assets/4a13c973-1295-4969-ae6c-8f04d943c34a" />
<img width="1455" height="684" alt="16" src="https://github.com/user-attachments/assets/e3b27743-0a92-41ce-b6bd-3df4ccbca78d" />

### Identifying the Payload: I discovered a file listed with the name AClient.exe

### To understand how the initial infection was staged, I pivoted to investigate the second execution parent identified in our previous steps: the installer binary.

<img width="1455" height="684" alt="17" src="https://github.com/user-attachments/assets/bd59e51c-f888-4814-abb7-7c64420af69c" />
<img width="1455" height="684" alt="18" src="https://github.com/user-attachments/assets/60277a93-3dfd-4605-8582-e023b4371e6c" />
<img width="1455" height="684" alt="19" src="https://github.com/user-attachments/assets/04093fd9-95a7-46e8-b4e0-db23e3e637bd" />
<img width="1455" height="684" alt="20" src="https://github.com/user-attachments/assets/df44ba57-92f0-46fc-b6de-e084d9e2854f" />

### Analyzing Dropped Files: Under the Relations tab, I scrolled down to the Dropped Files section to analyze the software packages and scripts extracted and written to disk by this installer. Filtering for Malicious Artifacts: Out of the 33 dropped files, I identified the malicious payloads by checking their detection ratios. I then ordered them chronologically as they appeared from top to bottom: searchhost.exe (associated with a multi-malware builder file listed at the top), syshelpers.exe (our flagged payload carrying a 53/70 detection rate), nat.vbs (a malicious VBScript carrying a 10/60 detection rate), runsys.vbs (another malicious script carrying a 3/62 detection rate)

### To answer the next question, I needed to locate the exact title of the threat intelligence report that detailed these specific indicators of compromise (IOCs)

<img width="1455" height="684" alt="21" src="https://github.com/user-attachments/assets/2e3c7478-78f9-4d0f-8ff7-34d8cc83e64a" />
<img width="1455" height="684" alt="22" src="https://github.com/user-attachments/assets/0401c47a-0359-4c28-bbf6-6bc78bd392b9" />
<img width="1455" height="684" alt="23" src="https://github.com/user-attachments/assets/1cfabb6d-6509-4d86-9a2e-5851479af7d1" />
<img width="1455" height="684" alt="24" src="https://github.com/user-attachments/assets/b7bd6838-efd0-44d4-b140-e78f4dad2079" />
<img width="1455" height="684" alt="25" src="https://github.com/user-attachments/assets/060f19c6-4c54-4f75-8378-f2c38d547e0e" />

### Scrolling through the community comments, I found a post by the user rectifyq. This post linked directly to a Check Point research article discussing these exact IOCs. Extracting the Report Title: The comment explicitly listed the metadata for the report:From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery

### With the threat intelligence report in hand, I analyzed the specific tools used by the attackers during the post-exploitation phase.

<img width="1455" height="684" alt="26" src="https://github.com/user-attachments/assets/9f3bb69f-1d70-4028-b03b-3f43478f4bfc" />
<img width="1455" height="684" alt="27" src="https://github.com/user-attachments/assets/6939ae8d-a991-4617-bbba-7baa4e4227d4" />

### The report highlighted a specialized credential-theft tool used to extract sensitive data directly from memory.

### Next, I looked into the social engineering vector the threat actors used to trick users into running the initial malicious payloads.

<img width="1455" height="684" alt="28" src="https://github.com/user-attachments/assets/d736e5f4-5a6a-4e0a-b256-5b460f66cebb" />

### This widely recognized phishing tactic is known as ClickFix (where victims are prompted to copy-paste a malicious PowerShell command into their run prompt).

### The final question required identifying the external platform utilized by the attackers to host their redirection infrastructure and guide victims to the malicious servers.

<img width="1455" height="684" alt="29" src="https://github.com/user-attachments/assets/def19a7a-2ada-41d7-bfcb-ba98ed5ab2e9" />

### The platform used to host these redirects was Discord.

### I submitted Discord as the final answer, successfully completing all tasks in the room!

<img width="1455" height="684" alt="30" src="https://github.com/user-attachments/assets/4523c8ef-ee17-4ed2-8005-1e8584eee965" />





















