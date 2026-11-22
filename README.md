# Banner Correlator (`banner-correlator`)

**Authorized Network Inventory, Service Banner Analysis, TLS Certificate Hygiene & Local Advisory Matcher**

`Banner Correlator` is a modular Python utility built for authorized asset discovery, service banner inspection, HTTP header hygiene analysis, TLS certificate checks, and local rule-based version correlation[span_0](start_span)[span_0](end_span)[span_1](start_span)[span_1](end_span)[span_2](start_span)[span_2](end_span)[span_3](start_span)[span_3](end_span)[span_4](start_span)[span_4](end_span)[span_5](start_span)[span_5](end_span). It is designed to run smoothly on desktop/server environments as well as **Termux** on Android[span_6](start_span)[span_6](end_span)[span_7](start_span)[span_7](end_span).

---

## Legal Notice & Responsible Use

This tool is intended strictly for authorized system administration, asset auditing, network inventory, and educational lab environments where you have explicit permission to inspect targets[span_8](start_span)[span_8](end_span). **You are solely responsible for how you use this software. The author assumes no liability or responsibility for unauthorized scanning, access, service disruption, misuse, or any resulting damages[span_9](start_span)[span_9](end_span).**

---

## Key Features

* **Asynchronous Network & Protocol Inspection (`scanner.py`):** Uses `asyncio` with configurable concurrency (`--concurrency`), random jitter (`--jitter-ms`), round-trip time (`rtt_ms`) measurement, and basic UDP probing (`--udp-ports`)[span_10](start_span)[span_10](end_span)[span_11](start_span)[span_11](end_span).
* **TLS / SSL Handshake & Certificate Inspection:** Checks TLS version, cipher suite, certificate byte size, expiration date (`notAfter`), and expired certificate status on TLS ports (`443`, `8443`, `993`, `995`, `465`)[span_12](start_span)[span_12](end_span).
* **Flexible Target & Port Management (`config.py`):** Supports single hosts/IPs, IPv6, CIDR ranges (e.g., `192.168.1.0/30`), target files (`--targets-file`), port ranges (`8000-8002`), and preset profiles (`web`, `ssh-ftp`, `mail`, `db`, `common`)[span_13](start_span)[span_13](end_span)[span_14](start_span)[span_14](end_span)[span_15](start_span)[span_15](end_span).
* **Banner, Version & HTTP Hygiene Parsing (`parsers.py`):** Detects product versions (such as OpenSSH, nginx, Apache, PostgreSQL, MariaDB, MySQL, Redis, Postfix, and ProFTPD) and inspects HTTP responses for missing security headers (`Strict-Transport-Security`, `Content-Security-Policy`, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`) and `Server` / `X-Powered-By` version disclosures[span_16](start_span)[span_16](end_span).
* **Operator-Based Rule Engine & Hygiene Score (`rules.py`):** Evaluates versions using `<`, `<=`, `==`, `!=`, `>=`, and `>` operators with severity levels (`critical`, `high`, `medium`, `low`, `info`), CVSS values, ignore lists, hardening recommendations, and a **0–100 hygiene score**[span_17](start_span)[span_17](end_span)[span_18](start_span)[span_18](end_span)[span_19](start_span)[span_19](end_span).
* **Multi-Format Reporting, Diffing & SQLite History (`reporter.py`, `storage.py`):** Exports results as terminal tables (`table`), `json`, Markdown (`md`), `csv`, and standalone `html` reports, compares runs against previous JSON reports (`--diff`), and stores scan history in SQLite (`--sqlite`)[span_20](start_span)[span_20](end_span)[span_21](start_span)[span_21](end_span)[span_22](start_span)[span_22](end_span).
* **Testing & CI (`tests/`, `mock_server.py`, `.github/workflows/ci.yml`):** Includes offline loopback mock services, unit tests, and a GitHub Actions workflow[span_23](start_span)[span_23](end_span)[span_24](start_span)[span_24](end_span)[span_25](start_span)[span_25](end_span)[span_26](start_span)[span_26](end_span).

---

## Project Structure

```text
banner-correlator/
├── .github/workflows/ci.yml      # GitHub Actions rule validation and test workflow
├── banner_correlator/
│   ├── __init__.py               # Package metadata and version
│   ├── cli.py                    # CLI argument parsing, execution flow, and exit codes
│   ├── config.py                 # Config loading, target/CIDR expansion, and port profiles
│   ├── mock_server.py            # Local loopback mock services for offline testing
│   ├── parsers.py                # Banner version parsing, HTTP hygiene, and hex preview
│   ├── reporter.py               # Table, JSON, Markdown, CSV, HTML output and diff engine
│   ├── rules.py                  # Rule loader, version comparison, ignores, and scoring
│   ├── scanner.py                # Async TCP, TLS, and sync UDP scanning routines
│   └── storage.py                # SQLite scan history persistence
├── tests/
│   └── test_correlator.py        # Unit and end-to-end tests
├── config.example.json           # Sample JSON configuration file
├── correlator.py                 # Main CLI entry point
├── cve_rules.json                # Sample advisory and version rules
├── CHANGELOG.md                  # Release notes
├── LICENSE                       # MIT License
└── README.md                     # Documentation
. 
