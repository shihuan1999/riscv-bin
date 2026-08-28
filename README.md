# riscv-bin

Prebuilt binaries (bottles) for OpenHarmony RISC-V64 musl, used as the official bottle source for [Harmonybrew](https://github.com/Harmonybrew).

All bottles are attached to [GitHub Releases](https://github.com/shihuan1999/riscv-bin/releases) and built on snode7 with the spacemit musl-riscv64 toolchain, targeting **OpenHarmony 6.1 LTS**.

---

## 📦 Available Packages

### System Utilities (13)
- **bash** 5.3 - GNU Bourne-Again SHell
- **busybox** 1.37.0 - Lightweight Unix utilities
- **coreutils** 9.11 - GNU core utilities (ls, cp, mv, etc.)
- **diffutils** 3.12 - File comparison utilities (diff, cmp, etc.)
- **gawk** 5.3.2 - GNU awk, pattern scanning language
- **grep** 3.12 - GNU pattern search tool
- **gzip** 1.14 - GNU compression utility
- **less** 704 - Terminal pager
- **patch** 2.8 - Apply diff patches
- **sed** 4.10 - Stream editor
- **tar** 1.35 - Archive utility
- **tree** 2.1.1 - Recursive directory listing
- **zsh** 5.9 - Z shell

### Development Tools (5)
- **m4** 1.4.20 - Macro processor
- **make** 4.4.1 - GNU build automation
- **portable-git** 2.54.0 - Git version control (self-contained, 532MB)
- **portable-ruby** 4.0.3 - Ruby interpreter (Homebrew internal, 64MB)
- **python** 3.11.4 - Python interpreter with pip (13MB)

### Editors (2)
- **nano** 8.5 - Simple text editor
- **vim** 9.2.0150 - Vi IMproved editor

### Networking & Security (3)
- **curl** 8.19.0 - URL transfer tool with HTTPS support
- **dropbear** 2026.94 - Lightweight SSH server/client
- **openssh** 10.4p1 - OpenSSH client/server

### Libraries (3)
- **ncurses** 6.5 - Terminal UI library
- **openssl** 3.6.1 - Cryptography and SSL/TLS toolkit
- **zlib** 1.3.1 - Compression library

### Data Processing (2)
- **hello** 2.12.3 - GNU Hello (demo package)
- **jq** 1.8.1 - JSON processor ⚠️ *runtime dependency issue*

**Total: 28 packages**

---

## 🐍 Python Usage

After installing Python via Harmonybrew, you can use pip to install pure Python packages:

```bash
# Install Python
brew install hbrew/riscv/python

# Set up environment
export PYTHONHOME=/data/harmonybrew/Cellar/python/3.11.4
export PATH=/data/harmonybrew/Cellar/python/3.11.4/bin:$PATH

# Install pure Python packages with pip
python3.11 -m pip install pyyaml click jinja2 flask requests

# Test installation
python3.11 -c "import yaml; print('PyYAML works!')"
```

**Note on C-extension packages (numpy, pandas, etc.):**  
These require compilation from source. Since no prebuilt riscv64 musllinux wheels exist on PyPI, and the device lacks a full gcc toolchain, installing packages with C extensions is currently not supported. Focus on pure Python packages for best compatibility.

---

## 🚀 Quick Start

### 1. Install Harmonybrew on Device

```bash
# On your development machine
hdc shell -t <device-sn>

# On device (as root)
cd /data && wget https://github.com/Harmonybrew/harmonybrew/releases/latest/download/harmonybrew-riscv64.tar.gz
tar xzf harmonybrew-riscv64.tar.gz
. /data/harmonybrew/hbrew-env.sh

# Verify installation
brew --version
```

### 2. Install Packages

```bash
# Install a single package
brew install hbrew/riscv/curl

# Install multiple packages
brew install hbrew/riscv/bash hbrew/riscv/vim hbrew/riscv/tree

# List installed packages
brew list

# Search available packages
brew search
```

### 3. Use Installed Tools

```bash
# Example: curl with HTTPS
/data/harmonybrew/bin/curl https://github.com

# Example: vim editor
/data/harmonybrew/bin/vim file.txt

# Example: tree directory listing
/data/harmonybrew/bin/tree /data
```

---

## 🔧 Technical Details

### Build Environment
- **Toolchain**: spacemit musl-riscv64 (gcc 14.2.0)
- **Target**: OpenHarmony 6.1 LTS (musl libc)
- **Architecture**: riscv64
- **Build Server**: snode7 (internal)

### SSL/TLS Support
Harmonybrew includes a curated CA bundle at `/data/harmonybrew/etc/ssl/cacert.pem`. The brew shim automatically configures SSL_CERT_FILE for secure HTTPS downloads.

### Dependency Resolution
Harmonybrew automatically resolves and installs dependencies. For example:
```bash
brew install hbrew/riscv/curl
# Automatically installs: openssl, zlib
```

### Known Issues
- **jq**: Runtime linking errors due to missing libjq.so.1/libonig.so.5 (formula needs dependency fix)
- **openssh/coreutils**: Some binaries have musl vs glibc symbol compatibility issues (`__res_state`, `qsort_r`)

---

## 📖 Formula Source

All formula definitions are maintained at:  
🔗 https://github.com/shihuan1999/homebrew-riscv

To use this tap:
```bash
brew tap hbrew/riscv https://github.com/shihuan1999/homebrew-riscv
```

---

## 🤝 Contributing

To add more packages:

1. **Write a formula** in `homebrew-riscv/Formula/<initial>/<name>.rb`
2. **Build on snode7** using the build script
3. **Upload bottle** to this repository's releases
4. **Update formula** with bottle sha256 and GitHub root_url
5. **Submit a PR** to homebrew-riscv

For detailed porting guide, see the [Harmonybrew documentation](https://github.com/Harmonybrew/harmonybrew).

---

## 📜 License

All bottles are built from open-source software with their respective licenses. The packaging infrastructure is MIT licensed.

Built with ❤️ for the OpenHarmony RISC-V64 community.
