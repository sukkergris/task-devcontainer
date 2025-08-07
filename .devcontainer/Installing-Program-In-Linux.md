# Installing Programs in Linux

This document explains common methods for installing programs in Linux environments, with a specific focus on container environments like Alpine Linux.

## Common Installation Methods

### 1. Package Managers

Package managers are the primary way to install software in Linux distributions:

- **Alpine Linux**: `apk`
  ```bash
  apk update
  apk add package-name
  ```

- **Debian/Ubuntu**: `apt`
  ```bash
  apt update
  apt install package-name
  ```

- **Red Hat/Fedora**: `dnf` or `yum`
  ```bash
  dnf install package-name
  ```

### 2. Installation Scripts

Many projects provide installation scripts that handle downloading and installing the software.

#### Example: Installing Taskfile

```bash
curl -sL https://taskfile.dev/install.sh | sh -s -- -d -b /usr/local/bin
```

Breaking down this command:

- `curl -sL https://taskfile.dev/install.sh`: Downloads the installation script
  - `-s`: Silent mode (no progress or error messages)
  - `-L`: Follow redirects if the URL has moved

- `|`: Pipes the downloaded script directly to the shell

- `sh -s -- -d -b /usr/local/bin`: Executes the script with specific options
  - `sh`: The shell that will run the script
  - `-s`: Read from standard input (the piped content)
  - `--`: Separates shell options from script options
  - `-d`: Download mode (download binary without further installation steps)
  - `-b`: Specify the binary directory
  - `/usr/local/bin`: Target directory (on system PATH)

### 3. Manual Installation

For programs without package manager entries or installation scripts:

1. **Download** the program (using `wget` or `curl`)
2. **Extract** if needed (`unzip`, `tar`)
3. **Compile** if needed (may require `gcc`, `make`, etc.)
4. **Install** to an appropriate directory
5. **Add to PATH** if installed in a non-standard location

## Installation Locations

### System-wide Installation

- `/usr/local/bin`: Standard location for manually installed programs
- `/opt`: Often used for self-contained program installations
- `/usr/bin`: Used by package managers (avoid manual installations here)

### User-specific Installation

- `~/bin` or `~/.local/bin`: User's personal bin directory
- Requires adding to PATH in `.bashrc`, `.profile`, etc.

## Container Considerations

In Docker/container environments:

- Prefer system-wide installations in `/usr/local/bin`
- Install as root during image building
- Switch to non-root users after installation when needed
- Use multi-stage builds to keep images small

## Environment Variables

Sometimes you need to set environment variables after installation:

```dockerfile
ENV PATH="/some/custom/path:${PATH}"
```

## Verifying Installations

Always verify installations:

```bash
command -v program-name    # Check if command exists
program-name --version     # Check version
```