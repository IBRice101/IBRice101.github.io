# [APT Talk Title Here]

By Isaac Basque-Rice

Talk given 2022-11-09

## Introduction

If you're anything like me, one of the biggest reasons you got interested in cybersecurity is because of these massive, sophisticated, James Bond style attacks you'd heard about in the news, you know, Stuxnet, WannaCry, the US Election tampering stuff. But have you ever wondered who actually did any of this?

Obviously the answers here are countries (or nation-states), you're likely to be thinking "North Korea", "Russia", "China", "The US", and other such entities. And you'd be right! But Who *specifically* does all this crazy stuff? What groups? How do they do it? And why?

The answer to these questions and more is what I'll be going over in this talk. So without any further ado, let's learn a little bit about Advanced Persistent Threats.

## What Are APTs?

APTs, or Advanced Persistent Threats are, as the name suggests, pretty bloody dangerous guys! Let's break down what each of these words actually mean

- Advanced: capable of sophisticated attacks, normally using malware, on targets of great importance
- Persistent: They just keep going, this in my view is the secret to their success, they have the resources, time, and backing to be able to continue going with an attack against their target for as long as they need. If you're in the sights of an APT there's simply no point trying to stop them
- Threats: Pretty self explanatory, they have the capability to inflict massive amounts of damage on their target

When you put all of these together, it becomes clear that they aren't your run-of-the-mill hackers. They almost always have the backing of some nation state actor or another, often being sections of the militaries and/or security services of respective nation states. This is to such an extent that many militaries around the world, including the UK’s Armed Forces, consider the cyber space to be the fifth dimension of warfare (after land, sea, air, and space). In fact, the term “APT” was coined by the US Department of Defense [sic] in the late 2000s to “describe cyberespionage efforts by North Korea against American national security interests”

How, though, do we know about APTs? How do we know which ones do what, and how can they be identified in specific cases? The answer to these questions is the MITRE ATT&CK framework

## MITRE ATT&CK

Before we get into the meat of the talk, the tactics the APT groups use, and what they've actually done in the past (some really cool stories), it's important to know how we can learn about these groups, both for an interested amateur, such as myself and most of the people here (I imagine), wishing to do research, and from a blue team perspective, so they know what they're up against when an APT comes knocking.

This is where the ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) framework comes in. This framework, developed by a non-profit research organisation called MITRE (who are also very highly regarded in the Cybersecurity field for helping maintain the Common Vulnerabilities and Exposures list, or CVEs), is a curated database of information around adversary groups of all flavours, including, of course, APTs, but also Ransomware groups and other such absolute pests. The framework, [available here](https://attack.mitre.org/), provides a categorised list of known tactics and techniques used by different groups in a "Matrix", as well as more in-depth information about specific tactics, techniques, data sources, mitigations, groups, software, and other resources on their website. I'd really recommend going over there if anything I'm about to say in this talk interests you

Be under no illusion that this not is one of the most helpful tools out there to counter adversary groups. The information available here, before 2013 when the framework was released, was previously only really available to elite incident responders and classified documents. This kind of thing really embodies one of my favourite things about tech, free and open collaboration for the benefit of everyone.

The benefit it and tools like it bring to both blue and red teamers is pretty brilliant, It is designed to fit perfectly into individual organisations threat models kind of like a defence methodology, whilst also allowing red teamers both within that organisation and externally to use it as a jumping off point or even full attack methodology, allowing organisations to see where their security holes are, you know, basic red team shit.

Let's go over the Tactics Techniques and Procedures outlined in the ATT&CK framework.

## Tactics, Techniques, and Procedures

TTPs, or Tactics, Techniques, and Procedures, are a crucial concept in cyber security (and terrorism) education. They serve to outline sets of behaviours so a specific action can be mapped to a malicious actor or group. MITRE outlines a set of 14 tactics, which are divided into 222 techniques, many of which have several sub-techniques themselves. 

In this section of the talk I won't go over literally *everything* because we'd be here all day and you'd be asleep in your chairs, but I will go over one or two of the most interesting techniques that form part of each of the tactics.

To access the Matrix itself, visit https://attack.mitre.org/matrices/enterprise/

The 14 tactics are:

1. **Reconnaissance**: AKA Information gathering, used to plan future operations
   1. Active Scanning: includes Scanning IP Blocks, Vuln Scanning, and Wordlist Scanning. Can be performed in numerous, often native ways such as ICMP (ping), UDP, TCP, all often done through nmap. Is often a precursor to other kinds of recon such as open website/domain search and open technical database search. Not really able to mitigate this because it's based on stuff done outside of the reach of individual orgs' security team, just make sure all of your important stuff is not available through active scans, p obvious really
2. **Resource Development**: Establishing resources they can use to support operations through the creation, purchasing, or theft of resources (such as tools, infrastructure, accounts)
   1. Develop Capabilities: Includes malware, code signing certificates, digital certificates, and exploits. APTs do this quite often, Specifically in malware, which was the focus of my mini project (and possibly dissertation who knows). An APT can create malware, sign the code to make it look like a legitimate executable (ex. Microsoft DLLs), and also discover and exploit new 0day vulns, like EternalBlue
3. **Initial Access**: When they're trying to get into the network
   1. Phishing: Includes spear-phishing by attachment, link clicking, or third party services. Electronically delivered social engineering, classic email scams, but often *remarkably* effective
4. **Execution**: AKA remote code execution (RCE), results in often malicious code developed by the attacker running on a target machine
   1. User Execution: Getting the user to deploy code, often via phishing, using malicious files, links, and so on
   2. Command and Scripting Interpreter: Such as Powershell on Windows or a Unix Shell on Linux. Using various scripts (possibly run immediately) to execute payloads and such
5. **Persistence**: The process of maintaining a foothold on a network
   1. Boot or Logon Initialization Scripts/Autostart Execution: These are actually two separate things in the Matrix but I thought seen as how they're so similar I may as well bunch them together. Can configure an operating system to automatically run a script or executable on system start or reboot in order to maintain presence in a network, this can be done in many different ways including editing the registry (where info and settings for software, hardware, and the system itself is stored), injecting a module into the kernel itself, or editing various system or network wide logon scripts.  
6. **Privilege Escalation**: Gaining higher level permissions, with the aim of admin or root
   1. Access Token Manipulation: Access Tokens are granted to all running processes under Windows to inform the OS what level of privilege the user who began the process has. In many cases however these can be modified to make a process, such as a reverse shell, appear as if it is running under the admin level privilege. This can be performed by stealing or impersonating an existing token on the machine, creating a new token, or spoofing the Parent Process Identifier (PPID).
   2. Boot or Logon Initialisation Scripts and Autostart Execution can also be used for PrivEsc
7. **Defense [sic.] Evasion**: Avoiding being detected
   1. Access Token Manipulation can also be used here
   2. Process Injection: malware or other bad actors' code can be ran inside the process space of another program in a process called "injection", this allows the malicious code to piggyback off of the legitimate software, including privileges and all, without being detected as easily as it is attached to something legitimate. This can be done by injecting DLLs, PE (Portable Executable files, the category to which EXEs belong), hijacking thread executions, and many more methods
   3. There are absolutely loads more of these, I encourage you to read up yourself
8. **Credential Access**: Stealing account names and passwords
   1. Credentials from Password Stores: Often specific operating systems have certain places in which credentials are stored, in Windows this is the Credential Manager, and in Linux this is `/etc/passwd` and `/etc/shadow`. Many organisations also employ third party password managers, which may be simple to access given only one (often insecure) password stands between an attacker and total access. This technique often goes hand in hand with brute forcing, which is another valid, albeit slow approach.
   2. Input Capture: Another method, possibly using keylogging, GUI or Web Portal capture, or other means. Can be very effective, also.
9.  **Discovery**: 
   1. 
10. **Lateral Movement**:
   1. 
11. **Collection**:
   1. 
12. **Command and Control**:
   1. 
13. **Exfiltration**:
   1. 
14. **Impact**:
   1. 

## Case Studies

### APT 28



### APT 38



### Equation Group
