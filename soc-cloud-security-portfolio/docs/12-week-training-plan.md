# Next Phase: SOC Analyst & Cloud Security

A 12-Week Hands-On Lab & Portfolio Plan

Prepared for Adrian  |  Post-Security+ Training Roadmap

## How This Plan Works

Security+ is done — congratulations on passing. This phase is deliberately hands-on: no more exam-cram studying. Each week pairs a free lab platform with a course you already own on Udemy, and ends with something concrete — a lab built, an alert investigated, or a write-up drafted. By week 12 you'll have a running home lab and 2-3 portfolio investigation reports you can link from your resume and LinkedIn.

Suggested pace: 12-15 hours/week, split across labs, your Udemy courses, and continued job applications — keep applying throughout, don't wait until the end of the 12 weeks. Adjust the pace around your schedule; the sequencing matters more than the exact days.

## Core Free Platforms You'll Use

| Platform | Why it's here | Cost |
|---|---|---|
| TryHackMe — SOC Level 1 path | Structured curriculum: alert triage, SIEM (Splunk/Elastic), phishing analysis, MITRE ATT&CK, kill chains. The backbone of weeks 3-6 and 10. | Free tier covers most of it |
| LetsDefend | Realistic SOC alert-triage simulator (now part of Hack The Box). Free tier gives 5 alerts/month — pace yourself. | Free tier |
| CyberDefenders | Free blue-team CTF challenges, including cloud forensics scenarios using GuardDuty, CloudTrail, and Entra ID logs. | Core catalog free |
| Blue Team Labs Online (BTLO) | Gamified investigation challenges, good for weeks 10-11 once you have fundamentals down. | Free tier |
| Wazuh | Free, open-source SIEM/XDR with no daily ingestion cap. Runs alongside Splunk in your home lab so you can compare automated alerting against raw log search. | Free, self-hosted |
| Sigma | Not a platform — a vendor-agnostic rule format for writing detections once and mapping them to Splunk, Elastic, or Wazuh. Used in week 10 for your first hand-written detection. | Free / open spec |

# The 12 Weeks at a Glance

| Weeks | Phase | Focus |
|---|---|---|
| 1-2 | Foundations Refresh | Linux fluency + a home AD lab you'll reuse all cycle |
| 3-6 | SOC Analyst Core | Alert triage, SIEM (Splunk/Elastic), phishing analysis, detection frameworks |
| 7-9 | Cloud Security Foundations | Azure AD/Entra ID, Okta/SAML federation, AWS networking & cloud logging — including piping AWS logs into your home SIEM |
| 10-12 | Threat Hunting + Capstone | Custom detections, cloud investigations, portfolio write-ups, job-search push |

# Weeks 1-2: Foundations Sprint (Cyber Security 101 + Home Lab)

Updated from the original plan: you picked up a paid TryHackMe month and are moving at 4-5 hrs/day, and you're finding real gaps in Cyber Security 101 rooms beyond just Linux/Network/Windows Fundamentals. Doing the full path (~45 hours) is a reasonable call, not scope creep — the extra rooms cost nothing more on an active subscription, and discovering unfamiliar material is a good sign it's not just redundant review for you specifically. Two things to protect while you do it: don't let the AD lab build slip, since it's the log source everything from week 4 onward depends on — interleave it rather than pushing it to "after" the 45 hours. And don't treat 4-5 hrs/day as this plan's new permanent pace; it's a smart, deliberate sprint while the subscription clock is running, not a sustainable rate for all 12 weeks.

| Resource | What to do |
|---|---|
| TryHackMe (paid, 1 month) | Full Cyber Security 101 path (~45 hrs) at your own pace. This now includes Linux/Network/Windows Fundamentals plus the exploitation-focused rooms (Metasploit, EternalBlue, web hacking basics) that aren't SOC-specific but build real attacker-side context — understanding how an attack works makes you a sharper defender later. |
| Udemy — The Linux Command Line Bootcamp (Colt Steele) | Refresher only, done alongside the Linux Fundamentals rooms — grep, piping, and redirection sections. This course does not cover sed or awk — get those from a free reference (e.g. the Grymoire sed/awk guides) or check whether your "Linux Sysadmin: 5 Hands-On Projects" course covers them instead. |
| Udemy — Active Directory: Deploying and Managing Certificate Services | Interleave this with the THM sprint — don't wait until Cyber Security 101 is fully done. Build one Windows Server (Domain Controller + Certificate Services) VM and one Windows 10/11 client VM joined to the domain. This is the one piece with a hard downstream dependency (Splunk log ingestion in week 4), so it can't slip behind the THM grind. |
| Windows Event ID drill | Once your lab DC is up: in Event Viewer, generate and explain each of these: 4624 (successful logon), 4625 (failed logon), 4720 (user account created), 4726 (user account deleted), 4740 (account locked out). Know these cold — they come up constantly in SOC interviews and on the job. |
| Checkpoint | You've completed the full Cyber Security 101 path, you can navigate a Linux box blind and pipe a log through grep (and sed/awk, from whichever source you used), and you have a working AD domain in VirtualBox/VMware with at least one user account and one client joined — ready to be your log source for the next 8 weeks. |

# Week 3: SOC Fundamentals

| Resource | What to do |
|---|---|
| TryHackMe SOC Level 1 (free) | Junior Security Analyst Intro → SOC Role in Blue Team → Humans as Attack Vectors → Systems as Attack Vectors. |
| LetsDefend (free tier) | Create an account and work through your first 2-3 alerts. Free tier caps at 5/month, so don't burn them all in one sitting. |
| Checkpoint | You can describe, in your own words, what a Tier 1 SOC analyst's shift actually looks like — useful for interview answers. |

# Week 4: SIEM: Splunk, Elastic & Wazuh

*Splunk Free (500MB/day ingestion) is plenty for a home lab — no cost, just a signup. Wazuh has no daily cap and is a real free SIEM/XDR used in the industry, so it's worth running alongside Splunk rather than instead of it.*

| Resource | What to do |
|---|---|
| TryHackMe SOC Level 1 (free) | Introduction to SIEM → Splunk: The Basics → Elastic Stack: The Basics. |
| Home lab build | Install Splunk Free on a Linux VM. Install Sysmon + the Splunk Universal Forwarder on your Windows AD lab client. Get Windows Event Logs + Sysmon events flowing into Splunk. |
| Wazuh side-by-side | Install the Wazuh server (also free) and its agent on the same Windows client. Compare what Wazuh flags automatically against the raw Windows Event Logs sitting in Splunk — this is the fastest way to actually understand what a SIEM/detection layer adds over raw log storage. |
| Checkpoint | You can run a basic SPL search against your own lab's logs and find a specific login event, and explain one alert Wazuh generated that wasn't obvious from the raw log alone. |

# Week 5: Detection Frameworks & Threat Intel

| Resource | What to do |
|---|---|
| TryHackMe SOC Level 1 (free) | Pyramid of Pain → Cyber Kill Chain → Unified Kill Chain → MITRE → Introduction to EDR. |
| CyberDefenders (free) | Pick 1-2 beginner SOC/log-analysis challenges from the free catalog. |
| Checkpoint | You can map a given attack scenario to MITRE ATT&CK tactics/techniques without looking it up. |

# Week 6: Phishing Analysis & Reporting — Portfolio Piece #1

| Resource | What to do |
|---|---|
| TryHackMe SOC Level 1 (free) | Introduction to Phishing → Phishing Analysis Fundamentals → SOC L1 Alert Triage → SOC L1 Alert Reporting → SOC Workbooks and Lookups → SOC Metrics and Objectives. |
| LetsDefend (free tier) | Work a phishing-category alert with your remaining monthly credits. |
| Deliverable | Write a full incident report (executive summary, timeline, IOCs, analysis, remediation) on one alert you investigated this week. This is portfolio piece #1 — post it to a GitHub repo or a simple blog. |

# Week 7: Azure Identity & Entra ID

| Resource | What to do |
|---|---|
| Udemy — Azure Active Directory Masterclass (Kevin Brown) | Complete the course. Build a free-tier Azure tenant alongside it. |
| Udemy — Azure Administrator AZ-104 (Kevin Brown) | Start now, but prioritize the identity/security modules (Entra ID, Conditional Access, RBAC) over networking/storage — you can finish the rest later since a cert isn't the goal here. |
| Hands-on | In your free Azure tenant: enable MFA, configure a Conditional Access policy, and review the Entra ID sign-in logs. |
| Checkpoint | You can explain what a Conditional Access policy actually blocks/allows, and you've seen what a sign-in log entry looks like. |

# Week 8: Identity Federation — Okta, SAML & OAuth

*IAM/SSO knowledge is increasingly asked about in both SOC and cloud security interviews — this week converts your Okta courses into something demonstrable.*

| Resource | What to do |
|---|---|
| Udemy — Getting Started with Okta (labITout) | Complete the course, following along in a free Okta developer tenant. |
| Udemy — Master SAML 2.0 with Okta (Viraj Shetty) | Complete the course. Configure SAML SSO from your Okta tenant into a test application (Okta's own sample apps work fine). |
| OAuth vs. SAML | Sketch out the SSO/OAuth/SAML/federation flow by hand — who talks to whom, what token gets passed where. Interviewers often ask you to explain the difference; being able to draw it beats reciting a definition. |
| Hands-on | Review the Okta System Log for a login event and trace it end-to-end — this is the same skill as triaging a SIEM alert, just in a different tool. |
| Checkpoint | You can explain the SAML request/response flow and how it differs from OAuth on a whiteboard, and you've seen a real login trace in the Okta log. |

# Week 9: AWS Networking & Cloud Detection

Updated: this week now closes a real gap in the plan. Weeks 7-9 otherwise treat cloud work as standalone — investigated in each provider's own console, never actually reaching your home SIEM. That's a missed opportunity, since the whole point of week 4's Wazuh-vs-raw-log comparison was learning what a SIEM adds over looking at a console directly. The same lesson applies to cloud, and "I investigated this GuardDuty finding through my own Splunk instance" is a stronger portfolio story than "I looked at it in the AWS console." AWS specifically is worth wiring up (not Azure or Okta too — doing all three would add real setup overhead across already-packed weeks) because it's the platform you already have professional depth in from your AWS Cloud Support Engineer background, which makes the integration faster and the eventual interview story land harder.

| Resource | What to do |
|---|---|
| Udemy — AWS Certified Advanced Networking Specialty (Stephane Maarek) | Work through the VPC, Transit Gateway, and security groups/NACLs sections specifically — skip the exam-cram sections unless you decide later you want the cert. |
| Hands-on (AWS Free Tier) | Enable GuardDuty and CloudTrail on a personal AWS account. Trigger a benign finding (GuardDuty has sample findings you can generate) and investigate it in CloudTrail. |
| CloudTrail → home SIEM integration (new) | Pipe CloudTrail logs into your home lab instead of stopping at the AWS console. Two documented, standard paths — pick one: (1) Splunk Add-on for AWS, which pulls CloudTrail via an S3 bucket + SQS notification pipeline, or (2) Wazuh's native AWS module, built in directly. Either way, the goal is the same GuardDuty finding you triggered above showing up as a searchable event in your own SIEM, not just in the AWS console. |
| CyberDefenders (free) | One cloud-forensics challenge using GuardDuty/CloudTrail logs. |
| Checkpoint | You can pull an AWS API call chain from CloudTrail and explain what happened and who did it — and you can find that same event with a search in Splunk or Wazuh, not just in the AWS console. |

# Week 10: Threat Hunting Basics

| Resource | What to do |
|---|---|
| TryHackMe SOC Level 1 (free) | Remaining rooms: Summit → Eviction. |
| Blue Team Labs Online (free tier) | Start 1-2 investigation challenges. |
| Home lab | Write one custom Splunk detection against your own lab logs — e.g., a search that flags repeated failed logins followed by a success (brute-force-then-success pattern). |
| Sigma rule | Write that same detection logic as a Sigma rule (a vendor-agnostic YAML format). It's a more portable portfolio artifact than a raw SPL query — it demonstrates you understand the detection logic independent of any one tool, and it maps to Wazuh or Elastic just as easily as Splunk. |
| Checkpoint | You've written a detection query and a Sigma rule from scratch, not just followed a walkthrough. |

# Week 11: Cloud Investigation — Portfolio Piece #2

| Resource | What to do |
|---|---|
| CyberDefenders (free) | One Entra ID or Azure AD forensics challenge, plus one AWS GuardDuty challenge if you haven't already. |
| Review | Revisit anything from the Azure AD Masterclass / AZ-104 identity modules that felt shaky. |
| Deliverable (updated) | Write a second portfolio report, this time cloud-focused — but now built around an investigation through your own CloudTrail → Splunk/Wazuh pipeline from week 9, not just a CyberDefenders challenge. Walk through a GuardDuty finding as seen in your own SIEM: the alert, the underlying CloudTrail events, and your remediation reasoning. This is a noticeably stronger artifact to link from your resume than an investigation done entirely in someone else's platform. Post it alongside portfolio piece #1. |

# Week 12: Capstone & Job-Search Push

| Resource | What to do |
|---|---|
| Consolidate | Make sure your home lab (AD + Sysmon + Splunk + cloud logging) is in a demoable state — you'll want to talk through it in interviews. |
| Portfolio | Polish your 2-3 write-ups (phishing/SOC alert, cloud investigation, and optionally the threat-hunt detection you built in week 10). Host them on GitHub or a simple site. |
| Optional stretch | TryHackMe's SAL1 (Security Analyst Level 1) certification is a live, hands-on practical exam rather than a written test — worth considering if you want a credential that matches what you just built, without going back to multiple-choice studying. |
| Resume/LinkedIn | Add the home lab and portfolio links. Reframe bullet points around what you detected/investigated, not just what you studied. |
| Job search | Resume or continue applications with the portfolio links attached — SOC Analyst and Cloud Security roles both benefit from being able to point to real, working examples. |

## Optional: A Weekly Rhythm

If you find a loose 12-15 hrs/week hard to self-pace, here's an optional day-by-day rhythm you can drop over any week above — use it or ignore it:

- Mon: TryHackMe / free-platform lab work

- Tue: Linux or Udemy course work

- Wed: SIEM / detection work (Splunk, Wazuh, Sigma)

- Thu: Cloud / identity (AWS, Azure, Okta)

- Fri: Incident response practice + job applications

- Sat: longer 2-4 hour lab-build session

- Sun: review + LinkedIn/portfolio upkeep

## Beyond These 12 Weeks

Not part of this plan, just context for later: once you've landed a role, CySA+ and the AWS Security Specialty are the natural next certifications — both assume exactly the hands-on foundation this plan builds. No need to think about them now.

# A Few Notes

- You already have AWS SAA and Security+ — that's a real credential combination for cloud security roles. This plan is about proving you can use that knowledge, not replacing it.

- If SOC Analyst and Cloud Security start to clearly diverge in interest once you're in it, that's useful signal — let week 6-9 tell you which direction pulls harder, and lean into it for weeks 10-12.

- LetsDefend's free tier is capped at 5 alerts/month — it's built into weeks 3 and 6 deliberately so you don't burn through it early.

- The AWS AI Practitioner Udemy course isn't included above since it's not security-focused — keep it in your back pocket if you want a lighter-weight AWS cert later.

- Don't let the 12-week structure become another rigid study plan — the point of this phase is building things and getting comfortable being hands-on. If a week runs long, let it.

- Azure (week 7) and Okta (week 8) stay standalone, investigated in their own consoles — only AWS gets wired into the home SIEM, deliberately, to avoid adding integration overhead across three already-packed weeks.
