# routersploit


If you want **all commands in one sequence** for a fresh Kali Linux installation, use this:

```bash
# 1. Update Kali
sudo apt update

# 2. Install required packages
sudo apt install -y git python3 python3-pip python3-venv

# 3. Go to home directory
cd ~

# 4. Clone RouterSploit
git clone https://github.com/threat9/routersploit.git

# 5. Enter RouterSploit
cd ~/routersploit

# 6. Create virtual environment
python3 -m venv .venv

# 7. Activate virtual environment
source .venv/bin/activate

# 8. Upgrade pip
python -m pip install --upgrade pip

# 9. Install compatible Setuptools
python -m pip install "setuptools<81"

# 10. Install RouterSploit requirements
python -m pip install -r requirements.txt

# 11. Install telnetlib3 for newer Python versions
python -m pip install telnetlib3

# 12. Verify pkg_resources
python -c "import pkg_resources; print('pkg_resources OK')"

# 13. Verify telnetlib3
python -c "import telnetlib3; print('telnetlib3 OK')"

# 14. Start RouterSploit
python3 rsf.py
```

You should eventually get:

```text
rsf >
```

### Next time you want to start it

You **don't need to reinstall anything**:

```bash
cd ~/routersploit
source .venv/bin/activate
python3 rsf.py
```

If you want to make starting it even easier:

```bash
echo 'alias routersploit="cd ~/routersploit && source .venv/bin/activate && python3 rsf.py"' >> ~/.bashrc
source ~/.bashrc
```

Then simply:

```bash
routersploit
```

Only use RouterSploit against systems you own or have explicit authorization to test.








Yes. For GitHub, I’d make it a **clean setup/compatibility project** rather than uploading the entire RouterSploit source code. That also avoids unnecessarily republishing someone else’s repository.

Use this structure:

```text
routersploit-kali-setup/
├── README.md
├── setup.sh
├── requirements.txt
├── .gitignore
└── LICENSE
```

### `README.md`

# RouterSploit Kali Linux Setup

A simple setup guide and installation script for running [RouterSploit](https://github.com/threat9/routersploit) on Kali Linux.

This project helps install RouterSploit inside an isolated Python virtual environment and handles compatibility dependencies required by older RouterSploit code.

## Features

* Kali Linux setup
* Python virtual environment
* Compatible Setuptools version for `pkg_resources`
* `telnetlib3` support for newer Python versions
* RouterSploit installation from the official GitHub repository
* Simple launch command
* Keeps Python packages isolated from the system Python installation

## Requirements

* Kali Linux
* Python 3
* Git
* Internet connection
* `sudo` access

## Installation

Clone this repository:

```bash
git clone https://github.com/YOUR_USERNAME/routersploit-kali-setup.git
cd routersploit-kali-setup
```

Make the setup script executable:

```bash
chmod +x setup.sh
```

Run it:

```bash
./setup.sh
```

After installation:

```bash
cd ~/routersploit
source .venv/bin/activate
python3 rsf.py
```

You should see the RouterSploit prompt:

```text
rsf >
```

## Manual Installation

If you prefer to install everything manually:

```bash
sudo apt update
sudo apt install -y git python3 python3-pip python3-venv
```

Clone RouterSploit:

```bash
cd ~
git clone https://github.com/threat9/routersploit.git
cd routersploit
```

Create the virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install compatible dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install "setuptools<81"
python -m pip install -r requirements.txt
python -m pip install telnetlib3
```

Start RouterSploit:

```bash
python3 rsf.py
```

## Python Compatibility

Older RouterSploit code may use APIs that have changed or been removed from newer Python releases.

For example, older code imports:

```python
import pkg_resources
```

Therefore this setup uses:

```text
setuptools<81
```

RouterSploit can also fall back to:

```python
import telnetlib3 as telnetlib
```

when the old Python `telnetlib` module is unavailable.

## Troubleshooting

### `No module named pkg_resources`

Run:

```bash
python -m pip install "setuptools<81"
```

Verify:

```bash
python -c "import pkg_resources; print('pkg_resources OK')"
```

### `No module named telnetlib3`

Run:

```bash
python -m pip install telnetlib3
```

Verify:

```bash
python -c "import telnetlib3; print('telnetlib3 OK')"
```

### `externally-managed-environment`

Do not install the packages globally with `sudo pip`.

Activate the virtual environment:

```bash
cd ~/routersploit
source .venv/bin/activate
```

Then install the dependencies:

```bash
python -m pip install -r requirements.txt
```

### Start RouterSploit

```bash
cd ~/routersploit
source .venv/bin/activate
python3 rsf.py
```

## Usage

After starting RouterSploit:

```text
rsf >
```

You can use the framework's built-in commands to inspect available modules and configure authorized security testing.

Only test systems that you own or have explicit permission to assess.

## Project

This repository contains setup instructions and automation for Kali Linux.

RouterSploit itself is maintained in its own upstream repository:

[https://github.com/threat9/routersploit](https://github.com/threat9/routersploit)

## Disclaimer

This project is intended for authorized security research, education, penetration-testing labs, and systems for which you have permission to test.

The user is responsible for complying with all applicable laws, regulations, and authorization requirements.

## License

This setup material is provided under the MIT License.

### `setup.sh`

#!/bin/bash

set -e

echo "======================================"
echo " RouterSploit Kali Linux Setup"
echo "======================================"

if [ "$(id -u)" -eq 0 ]; then
echo "[!] Please run this script as a normal user."
echo "    The script will use sudo when required."
exit 1
fi

echo "[+] Installing system dependencies..."

sudo apt update
sudo apt install -y git python3 python3-pip python3-venv

ROUTERSPLOIT_DIR="$HOME/routersploit"

if [ -d "$ROUTERSPLOIT_DIR/.git" ]; then
echo "[+] RouterSploit already exists."
else
echo "[+] Cloning RouterSploit..."
git clone [https://github.com/threat9/routersploit.git](https://github.com/threat9/routersploit.git) "$ROUTERSPLOIT_DIR"
fi

cd "$ROUTERSPLOIT_DIR"

echo "[+] Creating Python virtual environment..."

if [ ! -d ".venv" ]; then
python3 -m venv .venv
fi

source .venv/bin/activate

echo "[+] Upgrading pip..."

python -m pip install --upgrade pip

echo "[+] Installing compatible Setuptools..."

python -m pip install "setuptools<81"

echo "[+] Installing RouterSploit requirements..."

if [ -f requirements.txt ]; then
python -m pip install -r requirements.txt
fi

echo "[+] Installing telnetlib3..."

python -m pip install telnetlib3

echo
echo "======================================"
echo " Installation completed"
echo "======================================"
echo
echo "Start RouterSploit with:"
echo
echo "cd ~/routersploit"
echo "source .venv/bin/activate"
echo "python3 rsf.py"
echo

### `requirements.txt`

# Compatibility dependencies for RouterSploit on modern Kali Linux

setuptools<81
telnetlib3

### `.gitignore`

# Python

**pycache**/
*.py[cod]
*$py.class

# Virtual environments

.venv/
venv/
env/

# Packaging

build/
dist/
*.egg-info/

# Environment / secrets

.env
.env.*
*.pem
*.key

# IDE

.vscode/
.idea/

# OS

.DS_Store
Thumbs.db

# Logs

*.log

### Create it on Kali

You can create the files directly:

```bash
mkdir -p ~/routersploit-kali-setup
cd ~/routersploit-kali-setup
```

Then create each file:

```bash
nano README.md
nano setup.sh
nano requirements.txt
nano .gitignore
```

Paste the corresponding contents above.

Make the script executable:

```bash
chmod +x setup.sh
```

Test it:

```bash
./setup.sh
```

Then create your GitHub repository and push:

```bash
git init
git add .
git commit -m "Add RouterSploit Kali Linux setup"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/routersploit-kali-setup.git
git push -u origin main
```

**One important point:** don't copy the entire `threat9/routersploit` source tree into your repository unless you have a reason to redistribute it. This setup repo can instead clone the upstream project during installation.
