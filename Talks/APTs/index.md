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

The 14 tactics are:

<!-- use https://attack.mitre.org/matrices/enterprise/ -->

1. Reconnaissance: AKA Information gathering, used to plan future operations
   1. Active Scanning: includes Scanning IP Blocks, Vuln Scanning, and Wordlist Scanning. Can be performed in numerous, often native ways such as ICMP (ping), UDP, TCP, all often done through nmap. Is often a precursor to other kinds of recon such as open website/domain search and open technical database search. Not really able to mitigate this because it's based on stuff done outside of the reach of individual orgs' security team, just make sure all of your important stuff is not available through active scans, p obvious really
2. Resource Development
3. Initial Access
4. Execution
5. Persistence
6. Privilege Escalation
7. Defense [sic.] Evasion
8. Credential Access
9.  Discovery
10. Lateral Movement
11. Collection
12. Command and Control
13. Exfiltration
14. Impact

## Case Studies

<!-- Some Info Here -->
