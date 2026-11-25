# Splunk WannaCry Ransomware Case Analysis

**SOC investigation and incident-analysis lab using Splunk, focused on reconstructing a simulated WannaCry ransomware outbreak from multiple log sources.**

> **Project status:** Lab instructions documented. Actual Splunk search results, patient-zero determination, impacted hosts, timeline, and screenshots still need to be added from the lab dataset.

## Case overview

The SOC receives alerts about abnormal SMB traffic across an internal network. Users then report inaccessible files and ransom notes, while IDS alerts suggest attempted exploitation of the SMBv1 vulnerability associated with **MS17-010 (EternalBlue)**.

The exercise asks the analyst to investigate the simulated incident in Splunk and determine:

- The suspected initial infected system (patient zero)
- The exploitation method
- The scope of impact
- The sequence of attacker actions

This is a simulated educational lab. The scenario and expected outcomes are not proof that a real WannaCry infection occurred.

## Lab objectives

- Identify ransomware-related SMB scanning.
- Detect EternalBlue exploitation alerts.
- Investigate suspicious Windows service installation.
- Identify suspicious process execution.
- Measure file-encryption activity across hosts.
- Review registry modifications for persistence.
- Build an incident timeline by correlating multiple log sources.

## Data sources

The lab states that the following log sources are pre-ingested and normalised:

- Windows Process Creation Logs
- Windows Service Installation Logs
- SMB Scanning Logs
- IDS Alerts (EternalBlue Detection)
- File Encryption Logs
- Windows Registry Modification Logs

**Lab index:** `wannacry_logs`

## Investigation workflow

1. Identify hosts generating unusually high SMB scan volumes.
2. Search IDS events for EternalBlue signatures and correlate source/destination IPs.
3. Investigate WannaCry-related service installation.
4. Review execution of named suspicious binaries.
5. Compare file-encryption event counts by host.
6. Inspect registry modifications, especially Run keys and service-related paths.
7. Build a timeline across the identified activity stages.
8. Validate each conclusion against the actual returned events before reporting it.

## Repository structure

```text
Splunk-WannaCry-Ransomware-Analysis/
├── README.md
├── documentation/
│   └── investigation-methodology.md
├── queries/
│   └── spl-queries.md
├── screenshots/
│   └── README.md
└── findings/
    └── investigation-summary.md
```

## Important analytical distinction

The lab instructions describe expected outcomes and provide investigation hypotheses. They do **not** include actual search output, timestamps, IP addresses, hostnames, or confirmed findings. Therefore this repository does not yet identify patient zero, confirm successful exploitation, or name impacted systems. Those conclusions must come from the Splunk evidence collected during the exercise.

In particular, the host with the highest SMB scan count is a **candidate** for patient zero, not definitive proof by itself. An IDS signature can indicate an exploitation attempt; successful exploitation requires corroborating evidence.

## Tools and technologies

- Splunk Enterprise
- Search Processing Language (SPL)
- Windows and network security event analysis
- Incident timeline reconstruction
- Git and GitHub

## Mitigation topics in the lab

- Apply the security updates addressing MS17-010.
- Disable SMBv1 where it is not required.
- Use network segmentation to limit lateral movement.
- Combine IDS signatures with behavioural detection.
- Maintain tested backup and recovery procedures.

## Responsible use and privacy

Before publishing screenshots or event examples, remove credentials, tokens, personal data, and sensitive internal host or network details. Publish only material you are authorised to share. Use original explanations and your own lab evidence rather than reproducing restricted course material.

## Disclaimer

This repository documents an educational, simulated case-analysis exercise. It is not a malware sample, a live incident report, or proof of compromise. All incident conclusions must be supported by the actual lab events.

## Author

**AliBen-git**  
Cybersecurity / SOC Analyst portfolio project
