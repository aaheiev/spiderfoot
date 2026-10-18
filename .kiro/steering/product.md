# Product Overview

SpiderFoot is an open source intelligence (OSINT) automation tool, version 4.0.0,
written in Python 3 and MIT-licensed. It gathers intelligence about a target from
a wide range of public data sources and makes the results easy to navigate.

## What it does

A scan is run against a single target entity. Supported target types include:

- IP address / IPv6 address
- Domain, sub-domain, and hostname
- Network subnet (CIDR) / ASN
- E-mail address, phone number, username, person's name
- Bitcoin address

Over 200 modules feed each other in a publisher/subscriber model: a module consumes
certain event types and emits new events, which other modules then consume. This
lets a single target expand into host enumeration, threat-intel lookups, breach
searches, metadata extraction, dark-web searches, and more.

## How it runs

- **Web UI**: `python3 ./sf.py -l 127.0.0.1:5001` starts the embedded CherryPy server.
- **CLI**: `sfcli.py` drives scans from the command line.
- **Storage**: SQLite back-end for scan data and custom querying.
- **Export**: CSV / JSON / GEXF.

## Correlation engine

SpiderFoot 4.0 introduced a YAML-configurable correlation engine. Rules live in
`correlations/*.yaml` (see `correlations/README.md` and `correlations/template.yaml`).
Rules describe how to find meaningful patterns across the collected events (for
example, an email appearing in multiple breaches, or an exposed database).

## Modules

Most modules do not require API keys; many that do offer a free tier. Modules fall
into categories such as Internal (pure analysis/extraction), Free API, Tiered API,
Commercial API, and Tool (wrappers around external binaries like Nmap, DNSTwist,
Nuclei, CMSeeK).

## Open source vs. HX

This repository is the open source edition. SpiderFoot HX is a separate commercial,
cloud-hosted product with additional capabilities. When discussing features, scope
them to this open source codebase unless the user explicitly asks about HX.
