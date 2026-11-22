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


## Generating & Opening an HTML Report Locally

`Banner Correlator` generates a self-contained, styled single-file **HTML report** using `--format html --output <file.html>`. The report includes scan timestamp, hygiene score, target host/IP, port, state, detected service, matched advisories, and banner details.

### 1. Generate the HTML Report (TCP + UDP Example)
```bash
python correlator.py --host example.com --ports 80,443 --udp-ports 53,123,443,8080 --format html --output example_report.html --progress
```

### 2. Copy to Android Downloads (Termux)
Once `termux-setup-storage` is configured, copy the report to your `Downloads` folder:
```bash
cp example_report.html ~/storage/downloads/
```

### 3. Open Locally
* **In Mobile Browser / Files App:** Open `file:///sdcard/Download/example_report.html` in your browser, or tap `example_report.html` inside Android's **Files** app under **Downloads**.
* **Via Local HTTP Server:** If your browser restricts local file URLs, start a temporary local server in Termux and open `[http://127.0.0.1:8080/example_report.html](http://127.0.0.1:8080/example_report.html)`:
  ```bash
  python -m http.server 8080 --bind 127.0.0.1
  ```
* **On Desktop:** Copy the file to your computer and double-click it or drag it into your browser window.

---

## CLI Reference

| Flag | Description |
| --- | --- |
| `--host` | Authorized host, IP, or CIDR (repeatable) |
| `--targets-file` | Path to a file containing newline-separated targets |
| `--ports` | Comma-separated TCP ports or ranges (e.g., `22,80,8000-8002`) |
| `--udp-ports` | Comma-separated UDP ports (e.g., `53,123`) |
| `--profile` | Preset TCP port profile (`web`, `ssh-ftp`, `mail`, `db`, `common`) |
| `--timeout` | Socket timeout in seconds (default: `2.0`) |
| `--concurrency` | Maximum concurrent probes (default: `25`) |
| `--jitter-ms` | Maximum random delay before each probe in milliseconds |
| `--rules` | Path to advisory rules JSON file (repeatable) |
| `--validate-rules` | Validate rule files and exit |
| `--config` | Path to JSON configuration file |
| `--format` | Output format (`table`, `json`, `md`, `csv`, `html`) |
| `--output` | Write rendered output to a file |
| `--sqlite` | Save scan history to a SQLite database file |
| `--diff` | Compare current results against a previous JSON report |
| `--only-matches` | Display only rows with findings or recommendations |
| `--mask-hosts` | Mask host IP octets in output |
| `--no-color` | Disable ANSI colors in table output |
| `--progress` | Display progress counter and percentage on `stderr` |
| `--fail-on-findings` | Exit with status code `1` if any rules match |

---

## Installation & Download Tutorial (Windows, Linux, Termux & Pardus)

Archive extraction structure: `banner-collerator.zip` -> `banner-collerator/` -> `banner-correlator-v3/` (project files: `correlator.py`, `banner_correlator/`, `cve_rules.json`, `tests/`).

### 1. Windows
1. Download `banner-collerator.zip` and extract it (e.g., to `C:\Users\<User>\Downloads\banner-collerator.zip`).
2. Open **PowerShell** or **Command Prompt** and enter the inner `banner-correlator-v3` directory:
   ```powershell
   cd "$env:USERPROFILE\Downloads\banner-collerator\banner-correlator-v3"
   python --version
   python -m unittest discover -s tests -v
   python correlator.py --host 127.0.0.1 --ports 22,80,443 --format table
   ```

### 2. Linux (Debian / Ubuntu / General)
1. Install `python3` and `unzip` if needed, extract `banner-collerator.zip`, and navigate to `banner-correlator/banner-correlator-v3`:
   ```bash
   sudo apt update && sudo apt install -y python3 unzip
   unzip ~/Downloads/banner-collerator.zip -d ~/
   cd ~/banner-collerator/banner-correlator-v3
   python3 -m unittest discover -s tests -v
   python3 correlator.py --host 127.0.0.1 --ports 22,80,443 --format table
   ```

### 3. Termux (Android)
1. Enable storage access (`termux-setup-storage`), install `python` and `unzip`, copy `banner-collerator.zip` from `Downloads`, extract it, and enter `banner-correlator/banner-correlator-v3`:
   ```bash
   termux-setup-storage
   pkg update && pkg install -y python unzip
   cp ~/storage/downloads/banner-collerator.zip ~/
   unzip ~/banner-collerator.zip -d ~/
   cd ~/banner-correlator/banner-correlator-v3
   python -m unittest discover -s tests -v
   python correlator.py --host 127.0.0.1 --ports 22,80,443 --format table
   ```

### 4. Pardus
1. Open a terminal, install `python3` and `unzip` via `apt`, extract `banner-collerator.zip`, and switch to `banner-correlator/banner-correlator-v3`:
   ```bash
   sudo apt update && sudo apt install -y python3 unzip
   unzip ~/Downloads/banner-collerator.zip -d ~/
   cd ~/banner-correlator/banner-correlator-v3
   python3 -m unittest discover -s tests -v
   python3 correlator.py --host 127.0.0.1 --ports 22,80,443 --format table
   ```


## License

Licensed under the **MIT License**. See `LICENSE` for details.


