# Investigation Methodology

## 1. Scope and scenario

This lab is a simulated SOC investigation of a WannaCry ransomware outbreak. The scenario begins with abnormal SMB traffic, followed by reports of inaccessible files and ransom notes. IDS alerts indicate possible exploitation of the SMBv1 vulnerability associated with MS17-010.

The investigation is intended to determine:

- The suspected initial infected system (patient zero)
- The method of exploitation
- The scope of impact
- The sequence of attacker actions

The exercise is described as intermediate and is estimated to take 60–90 minutes.

## 2. Data sources and environment

The lab states that its logs are pre-ingested and normalised in the Splunk index:

`wannacry_logs`

Available sources:

| Log source | Investigation use |
|---|---|
| Windows Process Creation Logs | Identify execution of suspicious binaries |
| Windows Service Installation Logs | Investigate possible service-based persistence |
| SMB Scanning Logs | Identify hosts generating high SMB scan volumes |
| IDS Alerts (EternalBlue Detection) | Find alerts matching EternalBlue signatures |
| File Encryption Logs | Estimate which hosts show the most encryption events |
| Windows Registry Modification Logs | Review possible registry-based persistence |

The lab description does not specify sourcetypes, a fixed time range, or the complete field schema. Validate those details in the actual Splunk environment before relying on the searches.

## 3. Investigation procedure

### Step 1 — Identify SMB scanning

Search for events where `event_type=SMBScan`, then count events by `src_ip` and sort from highest to lowest.

Look for internal IP addresses with unusually high scan counts. The lab proposes that the host with the highest count may be the suspected patient zero.

**Interpretation limit:** A high scan count makes a host a lead for further investigation. It does not, by itself, prove that the host was infected first or that it initiated the outbreak.

### Step 2 — Detect EternalBlue alerts

Search for IDS events whose `signature` contains `ETERNALBLUE`. Review event time, source IP, destination IP, severity, and `meta.evidence`.

Correlate the source IPs with the SMB scanning results. The lab says High or Critical severity alerts strongly indicate exploitation attempts.

**Interpretation limit:** An alert supports the presence of detected activity or an exploitation attempt. Confirming successful exploitation requires corroboration from endpoint or other relevant events.

### Step 3 — Investigate service installation

Search `ServiceInstall` events and filter for service names `WanaSvc` or names beginning with `WannaCry`. Review the timestamp, host, service name, binary path, user, and result.

Correlate service installation times and hosts with the IDS and process-creation events.

### Step 4 — Investigate process execution

Search `ProcessCreate` events for the process names `tasksche.exe`, `mssecsvc.exe`, or `mimikatz.exe`. Review the host, command line, user, and timestamp.

The lab advises focusing on uncommon process names executed from system directories and comparing execution times with service installation events.

### Step 5 — Measure file-encryption activity

Search `FileEncrypted` events and count them by host. Hosts with the largest counts are candidates for the most impacted systems in this dataset.

**Interpretation limit:** Event counts measure the records available to the search. They do not automatically establish the number of unique files encrypted, the full extent of damage, or whether every event represents successful encryption.

### Step 6 — Review registry modifications

Search `RegistryKeyModified` events and inspect `key_path`, `value_name`, `new_value`, host, user, and timestamp.

The lab specifically recommends reviewing Run keys and service-related registry paths. Validate the meaning of each change in its event context before describing it as persistence.

### Step 7 — Reconstruct the attack timeline

Use the event categories to group activity into scanning, exploitation, installation, execution, encryption, and persistence stages. Compare first-seen and last-seen times for each stage.

The supplied timeline search aggregates the earliest and latest event time per stage. This provides a stage-level overview, but it does not on its own prove the exact order of individual actions on one host. For a defensible incident narrative, retain host-level events and inspect their timestamps.

## 4. Evidence handling and reporting

For each search, record:

- The exact SPL used
- The time range selected in Splunk
- The returned event count
- Relevant fields and representative raw events
- Screenshots with sensitive information removed
- What the evidence supports and what remains uncertain

Keep observed facts separate from hypotheses. Do not state a patient-zero host, successful exploitation, or impacted-system list until the actual events support those conclusions.

## 5. Known limitations from the lab description

The supplied instructions do not contain actual search results or evidence. They do not identify:

- A patient-zero IP address or hostname
- Confirmed exploitation success
- Specific impacted systems
- Actual event timestamps or attack duration
- The exact number of encrypted files or affected endpoints
- The detailed sourcetype and field definitions

These details must be completed from the student's own Splunk investigation.
