# The Syscall Is Coming From Inside The House!: An Introduction to APTs

By Isaac Basque-Rice

Talk given 2022-11-30

[Slides Here](APT-Talk.pdf) 

**NOTE**: This talk was given on a Universities and Colleges Union strike day. Having spoken to a lecturer about this fact, they assured me that it would not be taken, at least by them, as strikebreaking. The Ethical Hacking Society is exclusively affiliated with the Abertay Students' Association and, as such, is not an academic institution and not involved directly in the strike. 

My full support and solidarity is with the UCU and their fight against a 25% real terms pay cut.

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

The tactics the APT groups use, and what they've actually done in the past (some really cool stories), it's important to know how we can learn about these groups, both for an interested amateur, such as myself and most of the people here (I imagine), wishing to do research, and from a blue team perspective, so they know what they're up against when an APT comes knocking.

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
   - Active Scanning: includes Scanning IP Blocks, Vuln Scanning, and Wordlist Scanning. Can be performed in numerous, often native ways such as ICMP (ping), UDP, TCP, all often done through nmap. Is often a precursor to other kinds of recon such as open website/domain search and open technical database search. Not really able to mitigate this because it's based on stuff done outside of the reach of individual orgs' security team, just make sure all of your important stuff is not available through active scans, p obvious really
2. **Resource Development**: Establishing resources they can use to support operations through the creation, purchasing, or theft of resources (such as tools, infrastructure, accounts)
   - Develop Capabilities: Includes malware, code signing certificates, digital certificates, and exploits. APTs do this quite often, Specifically in malware, which was the focus of my mini project. An APT can create malware, sign the code to make it look like a legitimate executable (ex. Microsoft DLLs), and also discover and exploit new 0day vulns, like EternalBlue
3. **Initial Access**: When they're trying to get into the network
   - Phishing: Includes spear-phishing by attachment, link clicking, or third party services. Electronically delivered social engineering, classic email scams, but often *remarkably* effective
4. **Execution**: AKA remote code execution (RCE), results in often malicious code developed by the attacker running on a target machine
   - User Execution: Getting the user to deploy code, often via phishing, using malicious files, links, and so on
   - Command and Scripting Interpreter: Such as Powershell on Windows or a Unix Shell on Linux. Using various scripts (possibly run immediately) to execute payloads and such
5. **Persistence**: The process of maintaining a foothold on a network
   - Boot or Logon Initialization Scripts/Autostart Execution: These are actually two separate things in the Matrix but I thought seen as how they're so similar I may as well bunch them together. Can configure an operating system to automatically run a script or executable on system start or reboot in order to maintain presence in a network, this can be done in many different ways including editing the registry (where info and settings for software, hardware, and the system itself is stored), injecting a module into the kernel itself, or editing various system or network wide logon scripts.  
6. **Privilege Escalation**: Gaining higher level permissions, with the aim of admin or root
   - Access Token Manipulation: Access Tokens are granted to all running processes under Windows to inform the OS what level of privilege the user who began the process has. In many cases however these can be modified to make a process, such as a reverse shell, appear as if it is running under the admin level privilege. This can be performed by stealing or impersonating an existing token on the machine, creating a new token, or spoofing the Parent Process Identifier (PPID).
   - Boot or Logon Initialisation Scripts and Autostart Execution can also be used for PrivEsc
7. **Defense [sic.] Evasion**: Avoiding being detected
   - Access Token Manipulation can also be used here
   - Process Injection: malware or other bad actors' code can be ran inside the process space of another program in a process called "injection", this allows the malicious code to piggyback off of the legitimate software, including privileges and all, without being detected as easily as it is attached to something legitimate. This can be done by injecting DLLs, PE (Portable Executable files, the category to which EXEs belong), hijacking thread executions, and many more methods
   - There are absolutely loads more of these, I encourage you to read up yourself
8. **Credential Access**: Stealing account names and passwords
   - Credentials from Password Stores: Often specific operating systems have certain places in which credentials are stored, in Windows this is the Credential Manager, and in Linux this is `/etc/passwd` and `/etc/shadow`. Many organisations also employ third party password managers, which may be simple to access given only one (often insecure) password stands between an attacker and total access. This technique often goes hand in hand with brute forcing, which is another valid, albeit slow approach.
   - Input Capture: Another method, possibly using keylogging, GUI or Web Portal capture, or other means. Can be very effective, also.
9.  **Discovery**: Figuring out the environment
   - Account Discovery: Including Local, Domain, Email, and Cloud accounts. This can be used to see what accounts can be utilised to aid in their next steps
   - Most of this section is using slightly varying techniques to discover as much information as possible, these include browser bookmarks, cloud infrastructure, debugger info (for evasion), file, directory, permissions, group policy, network services, and much more. Most of it, though, kind of speaks for itself
10. **Lateral Movement**: Moving through an environment
   - Remote Services: including Remote Desktop Protocol (RDP), Server Message Block (SMB), Secure Shell (SSH), and so on. This can result in malicious actors gaining access further into, or around, the network.
11. **Collection**: Gathering information of interest
   - Can collect data through many means, including MITM, where the adversary exists in between two data endpoints, discovery of information repositories such as code, an internal wiki, or SharePoint, as well as data that may be present on specific devices the attacker has accessed 
12. **Command and Control**: Attempting to communicate with compromised systems in order to control them
   - Application Layer Protocol: Malicious Actors can communicate using application layer protocols to avoid suspicion. Many C2 servers are configured with web pages to make use of web protocols. Chinese APTs have been observed recently with websites on the Uyghur genocide being hosted on their C2s. FTP, DNS, or Email servers also function for this purpose too.  
13. **Exfiltration**: Getting data out, stealing it from a target
   - Automated Exfiltration: Write an automated process to exfiltrate data, likely to the C2 server, after being gathered during collection. 
   - Exfiltration can be performed a number of ways, through both C2 and Non-C2 channels, to code repositories or cloud storage, or through physical medium such as a USB if the malicious actor has physical access to your internal network.
14. **Impact**: Attempting to manipulate, interrupt, or delete systems and data
   - Data Manipulation/Destruction: Inserting, deleting, corrupting, manipulating, and otherwise tampering with data stored on a network. This can be done to interrupt normal operation, ransom them, and so on

## Case Studies

This is the real meat of the talk, what have these groups actually *done*, who *are* they, and *why*?

At this stage in the talk it is important to note that all of this is merely *alleged*. Due to the sophistication of these actors it takes quite a lot of effort to prove these kinds of things beyond a reasonable doubt, and I also don't want to get in trouble with the respective governments and end up with some funny tasting tea.

### APT 28

Also known as Fancy Bear, Sofacy, Sednit, Tsar Team, STRONTIUM, and Pawn Storm. They're a Russian threat group that has likely been operating from the mid-2000s out of the Russian GRU (Main Intelligence Directorate) as Unit 26165. They primarily use spear-phishing and zero-days within malware in order to further their political aims. They have been associated with Russia as the attacks that have been attributed to them conveniently seem to follow the political aims of the Russian Federation remarkably closely, lol.

Arguably their most famous attack was the (successful) attempt to influence the 2016 United States Presidential Election by spear-phishing members of the Democratic National Convention, wherein tens if not hundreds of thousands of emails were leaked, thousands of credentials stolen, as well as other research documents etc. being breached and leaked. It was concluded that APT28 were behind the attack through forensic work carried out by CrowdStrike and backed up by the US Intelligence Community and Congress. The method of identification is, naturally, secret, but we do know that a break in their work JUST SO HAPPENED to coincide with a Russian holiday in honour of the military's electronic warfare services so... make of that what you will.

APT28 has also been linked to attacks on journalists in Russia, the US, Ukraine, Moldova, The Baltics, the parliament of Germany, ministries in the Netherlands, the International Olympic Committee, the Ecumenical Patriarchate, and, of course, Ukraine. Although the attacks on Ukrainian infrastructure have been remarkably light recently given current events, which not many analysts suspected (including several very smart people who gave talks at BSides Lisbon).

Fancy Bear make use of methods consistent with state actors. Spear phishing, malware drop websites, and zero days (up to 6 in some cases in one case in 2016!!!) are all cornerstones of their arsenals as previously mentioned. Their preferred method of attack is the following:

1. Leverage operational infrastructure (their server or a hijacked webmail server or something)
2. Craft an email with a malicious link, often with something like "YOUR EMAIL HAS BEEN COMPROMISED, CLICK HERE: https://bit.ly/TotallyNormalLink
3. Send to recipient
4. Enters credentials to spoof login portal
5. Creds sent to operational infrastructure and used to access targeted infrastructure
6. Drop malware
7. Move through
8. Gather and exfiltrate

These emails are primarily sent on mondays and fridays so they catch people off guard. They also have a few domains registered that resemble legitimate news sources (examples including bbcweather.org, politicweekend.com, and Georgia-travel.org) but link to sites that drop malware, often rootkits, onto the target computer. They have been known to route traffic through other compromised organisations as a sort of proxy. They tend to keep their malware extremely up to date so as to be avoided by AV software, returning to the environment to switch their implants, changing their command and control channels, and modifying their persistence methods. These changes can be done at blazing speed, in one instance the ADVSTORESHELL backdoor was implanted into a defence contractor's network, discovered by Kaspersky, removed, and then *patched and re-implanted within an hour and a half*.

### APT 38

APT 38, known also as Guardians of Peace (self-proclaimed during the Sony attack), God's Apostles, God's Disciples, Whois Team, HIDDEN COBRA, Zinc, and most infamously as Lazarus Group, is the second Advanced Persistent Threat group this talk will be analysing and is said to be affiliated to the Reconnaissance General Bureau (RGB) of the Democratic People’s Republic of Korea (North Korea). 

Lazarus Group are known for targeting financial institutions via the theft of credentials for the SWIFT monetary transaction system. Analysts have suggested that targeting financial institutions and cryptocurrency exchanges as they do is in order to raise funds for the North Korean state, as opposed to conducting legitimate trade on the global market. Arguably their most notable attack in this style was carried out against the Bangladesh Central Bank, during which $81 million USD was moved to four locations within the Philippines and a further $20 million USD was almost moved, were it not for a typo wherein an attacker misspelt “foundation” as “fandation”. So much for God's Chosen Ones I guess.

A series of cyber bank heists before and after the Bangladesh central bank heist has also been attributed to Lazarus due to shared patterns of activity and methods used by the respective group(s) that carried out this attack. These attacks were on Banco del Austro in Ecuador (January 2015), Philippines Bank (October 2015), an unsuccessful heist on Vietnam Tien Phong Bank (December 2015), and a bank in Poland (February 2017).

Lazarus Group’s most noteworthy and infamous alleged cyberattack is undoubtedly the 2017 WannaCry ransomware attack. Which saw the UK’s National Health Service IT system functionally unusable in many hospitals in the country, to the point they were forced to turn away patients. Other organisations affected included Telefonica (one of the largest telephone carriers in the world), a number of universities across China, arrival and departure screens on the German railway, and the interior ministry, railways, and many banks, in Russia were all affected. The demand for payment in bitcoin is consistent with the theory that “Use of ransomware to raise funds for the state [falls] under both North Korea’s asymmetric military strategy and ‘self-financing’ policy, and be within the broad operational remit of their intelligence services”.

Other attacks alleged to have been carried out by Lazarus that tie into the political aims of the DPRK include “Operation DarkSeoul”, which “crippled tens of thousands of computers in South Korea's banking and media sectors through the use of destructive malware” (Martin, 2015), the 2014 breach of Sony Pictures in response to their publication of a film about the assassination of the leader of North Korea, Kim Jong Un, and spear-phishing attacks on pharmaceutical companies, specifically Astra-Zeneca in late 2020 in response to an uptick in profit as a result of the COVID-19 pandemic.

### Equation Group

The final APT group in this talk is the EquationGroup, which is alleged by the Russian cybersecurity firm Kaspersky to belong to the Tailored Access Operations unit of the US National Security Agency (NSA). Kaspersky themselves consider the group to be able to execute some of the “most complex and sophisticated hacking techniques ever seen” and are responsible for malware such as Stuxnet and the related Flame malware which have the ability to bridge air gapped systems through a mix of advanced malware development techniques and sophisticated espionage. 

EquationGroup’s modus operandi is attacking critical national infrastructure such as Telecommunications, energy production (including oil and gas), and transportation, as well as military and nuclear research, companies in the financial, cryptographic, and aerospace sectors, mass media, and Islamic activists and scholars with a multitude of bespoke malware platforms. The extremely wide range of targets is another aspect of Equation that would point towards a highly sophisticated state actor funding this group. 

Arguably the most well-known cyber-attack EquationGroup have been involved in is the (alleged and as yet unconfirmed) [Stuxnet](../Stuxnet/index.md) malware, which is a malicious computer worm that targeted SCADA systems, particularly within the Natanz Nuclear Facility in Iran.  Stuxnet made use of four zero day vulnerabilities, including a windows rootkit, previously unknown antivirus evasion techniques, and stolen certificates from trusted certificate authorities. Stuxnet worked by first infiltrating an air-gapped computer system in Natanz, likely via USB, and then once it was in it would map out the internal network of the facility and subsequently slightly alter the rate at which the centrifuges that enrich the uranium in the facility spin, thus ensuring their eventual destruction. Interestingly, the malware would also feedback bogus data to the scientists at the facility so they would have no reason to suspect anything was wrong.

The extent to which the authors of the malware went in almost every regard, the usage of multiple zero days, the replaying of non-noteworthy data, has led some analysts to call Stuxnet “the best malware ever”.

This is, however, by no means the only accomplishment that the EquationGroup are renowned for. In 2016 a hacking group calling themselves “the Shadow Brokers” released the first of what would be, up until the writing of this report, five separate leaks of internal EquationGroup malware, tools, and exploits that they had been using to conduct their work. Most well-known of the leaks, leak number 5, entitled “Lost in Translation” contained, amongst other exploits, one known by the name “EternalBlue”, which has been used to significant effect by other APT groups (notably Lazarus with WannaCry. Additionally, in 2017 WikiLeaks leaked the existence of a reverse engineering tool developed and used within EquationGroup known as “Ghidra”, which the National Security Agency later released on a free and open source basis two years after, to great acclaim.

## Conclusion

Advanced Persistent Threats are, in my view and the view of many security professionals, *the* biggest threat to *any* organisation in the world. Unless you, yourself, are a state-level organisation or backed by one, if an APT decides to attack you, you're essentially already compromised. That is not to say, however, that we shouldn't learn from them. The techniques that they use are useful from both a defensive and offensive security perspective.

Naturally, eventually all of the stuff that these guys are doing will probably tickle down, if not to skids (like stuff that got leaked from the NSA did), then probably to advanced, but not particularly persistent threats. As such, looking at, and paying close attention to, APTs is the closest thing we most probably have to a time machine or magic mirror or something. 