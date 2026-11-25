# Investigation Summary – WannaCry Case Analysis

## Executive summary

The lab describes a simulated WannaCry ransomware outbreak involving abnormal SMB traffic, user reports of inaccessible files and ransom notes, and IDS alerts for possible exploitation of the SMBv1 vulnerability associated with MS17-010.

The investigation uses Splunk searches across SMB scanning, IDS alerts, Windows service installation, process creation, file encryption, and registry modification logs to reconstruct the activity.

**Evidence status:** The lab instructions and SPL searches have been supplied, but no actual Splunk result tables, event exports, timestamps, or screenshots have been provided here. This document therefore records the investigation questions and expected deliverables, not confirmed incident findings.

## Investigation questions and evidence required

| Question | Evidence to collect | Current status |
|---|---|---|
| Which host may be patient zero? | SMB scan counts by `src_ip`, correlated with earliest related events | Not yet determined |
| Was EternalBlue activity detected? | IDS signature, severity, source/destination IP, evidence, and corroborating endpoint events | Not yet validated |
| Was a suspicious service installed? | Service name, binary path, host, user, result, timestamp | Not yet validated |
| Were suspicious binaries executed? | Process name, command line, host, user, timestamp | Not yet validated |
| Which systems were impacted? | File-encryption event counts by host and supporting events | Not yet determined |
| Was registry persistence observed? | Registry key path, value name, new value, host, timestamp | Not yet validated |
| What was the attack sequence? | Correlated host-level events and timestamps across all stages | Not yet reconstructed |

## Expected investigation stages

The lab directs students to investigate these stages:

1. **Scanning:** Excessive SMB scanning activity.
2. **Exploitation:** IDS alerts matching EternalBlue.
3. **Installation:** Service installation events matching `WanaSvc` or `WannaCry*`.
4. **Execution:** Process creation for `tasksche.exe`, `mssecsvc.exe`, or `mimikatz.exe`.
5. **Encryption:** File-encryption event volume by host.
6. **Persistence:** Registry modifications, especially Run keys and service-related paths.

These are the stages the lab asks students to investigate. Their presence, order, and relationship must be established from the actual event data.

## Incident timeline

| Stage | First seen | Last seen | Host(s) | Evidence / notes |
|---|---|---|---|---|
| Scanning | To be collected | To be collected | To be collected | Pending Splunk results |
| Exploitation | To be collected | To be collected | To be collected | Pending Splunk results |
| Installation | To be collected | To be collected | To be collected | Pending Splunk results |
| Execution | To be collected | To be collected | To be collected | Pending Splunk results |
| Encryption | To be collected | To be collected | To be collected | Pending Splunk results |
| Persistence | To be collected | To be collected | To be collected | Pending Splunk results |

## Patient zero and impact assessment

- **Suspected patient-zero IP/hostname:** Not yet determined.
- **Evidence supporting patient zero:** Pending SMB scan results and event correlation.
- **Impacted systems:** Not yet determined.
- **Impact evidence:** Pending file-encryption event counts and supporting endpoint logs.
- **Exploitation assessment:** Not yet determined; IDS alerts must be reviewed and correlated.

Do not equate the highest SMB scan count with confirmed patient zero. Do not equate an IDS detection with proof of successful exploitation.

## Indicators of compromise to extract

Populate this section only with indicators actually present in the lab results:

- Source and destination IP addresses
- Relevant hostnames
- Service names and binary paths
- Process names and command lines
- Relevant registry paths and values
- Event timestamps and IDS signature details

## Mitigation discussion

The lab asks students to discuss these defensive controls:

- Apply updates addressing MS17-010.
- Disable SMBv1.
- Implement network segmentation.
- Use IDS and behavioural detection.
- Maintain backup and recovery planning.

A final recommendation should be tied to the observed evidence and the environment described by the exercise.

## Limitations

The supplied lab description does not include actual Splunk outputs, event timestamps, hostnames, IP addresses, confirmed patient zero, affected-system counts, or screenshots. No real-world infection or successful exploitation can be concluded from the scenario text alone.

## Lessons learned

Complete this section after running the searches. Explain what the returned events demonstrate about cross-source correlation, incident timeline construction, and the strengths and limitations of the available telemetry.
