# Splunk WannaCry Ransomware Case Analysis

**A hands-on Splunk Enterprise case-analysis lab focused on investigating WannaCry-related security events through log search, event correlation, and incident analysis.**

> **Project status:** Repository structure and project overview created. The detailed investigation, SPL queries, screenshots, and findings will be added after reviewing the original lab notes and evidence.

## Project overview

This repository documents a Splunk-based case analysis exercise concerning the WannaCry ransomware scenario. It is intended to demonstrate a structured approach to searching security data, identifying relevant events, examining timelines, and communicating investigation results.

The project is based on an educational lab. It should not be interpreted as evidence that a real WannaCry infection occurred in a production environment.

## Objectives

- Explore security-relevant events in Splunk Enterprise.
- Develop SPL searches to locate and summarize relevant activity.
- Examine event fields and timestamps to support investigation.
- Correlate related events where the available data supports it.
- Document evidence, observations, limitations, and lessons learned.
- Present the analysis in a clear and reproducible format.

## Tools and technologies

- Splunk Enterprise
- Search Processing Language (SPL)
- Security event and log analysis
- Git and GitHub

## Investigation workflow

1. **Understand the lab scenario** — document the exercise objective and available dataset.
2. **Validate the data** — identify the index, sourcetype, sources, relevant fields, and time range.
3. **Search for relevant events** — create focused SPL searches based on the actual data and scenario.
4. **Inspect and correlate evidence** — review raw events, timestamps, and related fields before drawing conclusions.
5. **Summarize findings** — distinguish observed evidence from hypotheses and explain limitations.
6. **Document lessons learned** — record what the investigation demonstrates and what additional evidence would be needed.

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

## Evidence and findings

The detailed SPL queries, screenshots, and findings are intentionally not pre-populated. They will be added after reviewing the original lab material so that the repository reflects the actual exercise rather than assumed indicators or invented results.

## Responsible use and privacy

Before publishing material, remove credentials, tokens, personal data, internal hostnames, private IP information where sensitive, and any restricted course or organisation content. Publish only work you are authorised to share. Use your own explanations and evidence; do not upload proprietary course answer keys or other people's work.

## Disclaimer

This is an educational portfolio project documenting a lab-based case analysis. It does not provide proof of infection, compromise, or attribution. Any conclusions must be supported by the logs and evidence included in the documented investigation.

## Author

**AliBen-git**  
Cybersecurity / SOC Analyst portfolio project
