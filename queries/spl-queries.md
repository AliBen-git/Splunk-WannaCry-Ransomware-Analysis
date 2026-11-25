# SPL Queries – WannaCry Case Analysis

The searches below are transcribed from the supplied lab description. They are the lab's starting queries, not independently validated results. Run them against the lab data, confirm that the fields exist, and record the actual output before drawing conclusions.

**Index:** `wannacry_logs`

## Task 1 — Identify suspicious SMB scanning activity

**Objective:** Find hosts generating unusually high SMB scan volumes.

```spl
index=wannacry_logs event_type=SMBScan
| stats count by src_ip
| sort -count
```

**Review:** Compare the scan counts by internal source IP. The highest-count host is a candidate for patient zero according to the lab guidance, but the count alone does not establish infection order.

**Record:** Top source IPs, event counts, time range, and any corroborating events.

## Task 2 — Detect EternalBlue exploitation attempts

**Objective:** Find IDS alerts matching the EternalBlue signature.

```spl
index=wannacry_logs signature="*ETERNALBLUE*"
| table _time src_ip dst_ip severity meta.evidence
```

**Review:** Examine severity and evidence details, then correlate source IPs with the SMB scanning results. The lab highlights High and Critical alerts as strong indicators of exploitation attempts.

**Record:** Alert timestamps, source and destination IPs, severity, evidence, and whether endpoint events corroborate the activity.

## Task 3 — Identify suspicious service installation

**Objective:** Find service installation events associated with the service names specified in the lab.

```spl
index=wannacry_logs event_type=ServiceInstall
| search service_name=WanaSvc OR service_name=WannaCry*
| table _time host service_name binary_path user result
```

**Review:** Inspect the service name, binary path, user, result, host, and timestamp. Correlate with process-creation and IDS events.

**Record:** Matching service events and whether installation succeeded, based on the `result` field's actual meaning in the dataset.

## Task 4 — Analyze suspicious process execution

**Objective:** Search for execution of the process names listed in the lab.

```spl
index=wannacry_logs event_type=ProcessCreate
| search process_name=tasksche.exe OR process_name=mssecsvc.exe OR process_name=mimikatz.exe
| table _time host process_name command_line user
```

**Review:** Inspect command lines, users, hosts, and timestamps. The lab recommends paying attention to uncommon process names executed from system directories and correlating them with service installation events.

**Record:** Process events and supporting context. A matching process name should be treated as an indicator to investigate, not as standalone proof of compromise.

## Task 5 — Detect file encryption activity

**Objective:** Compare the volume of file-encryption events across hosts.

```spl
index=wannacry_logs event_type=FileEncrypted
| stats count by host
| sort -count
```

**Review:** Identify hosts with the highest event counts and compare their activity with the other investigation stages.

**Record:** Counts by host and the search time range. Counts represent matching log events, not necessarily unique files or a complete damage assessment.

## Task 6 — Identify registry modifications

**Objective:** Review registry changes that may relate to persistence.

```spl
index=wannacry_logs event_type=RegistryKeyModified
| table _time host key_path value_name new_value user
```

**Review:** Focus on Run keys and service-related registry paths, as directed by the lab. Interpret each change using the surrounding event context.

**Record:** Relevant key paths, value names, new values, host, user, and timestamp.

## Task 7 — Build an attack-stage timeline

**Objective:** Summarise first-seen and last-seen times for the activity stages.

```spl
index=wannacry_logs
| eval stage=case(
    event_type="SMBScan","Scanning",
    signature="*ETERNALBLUE*","Exploitation",
    event_type="ServiceInstall","Installation",
    event_type="ProcessCreate","Execution",
    event_type="FileEncrypted","Encryption",
    event_type="RegistryKeyModified","Persistence"
)
| stats min(_time) as first_seen max(_time) as last_seen by stage
| convert ctime(first_seen) ctime(last_seen)
```

**Review:** Compare the first-seen and last-seen times for each stage.

**Important:** This is a stage-level aggregation across the selected dataset. It can help visualise the broad progression, but it does not prove a complete causal sequence or preserve host-by-host ordering. Inspect the underlying events when building the final incident timeline.

## Validation notes

- Confirm that the selected index contains the expected events.
- Check that the fields used in each search are populated and extracted as expected.
- Set and document an appropriate time range.
- If a search returns no results, investigate the index, field names, values, and time picker before concluding that the activity did not occur.
- The supplied Task 7 search uses a wildcard-style comparison in `case()`. Validate that the search behaves as intended in the lab's Splunk version; if it does not, document a tested alternative rather than silently changing the lab query.
- Capture actual results and screenshots before writing findings.

## Results log

For each task, add the date, time range, result count, key observations, screenshot filename, and interpretation. Do not fill in expected outcomes as if they were observed results.
