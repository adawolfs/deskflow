# Files and Directories Created by Deskflow

This document describes all files and directories that Deskflow creates during installation, configuration, and runtime.

## Overview

Deskflow creates various files and directories to store:
- User configuration and settings
- Server configuration
- Log files
- TLS/SSL certificates and security data
- Application state

The locations of these files vary by operating system.

## Platform-Specific Base Directories

### Windows
- **Settings Directory**: `%APPDATA%\Roaming\Deskflow\` (typically `C:\Users\<username>\AppData\Roaming\Deskflow\`)
- **Portable Mode**: `<install-path>\settings\` (when `Deskflow.conf` exists in install directory)
- **ProgramData**: `C:\ProgramData\Deskflow\` (used for system-wide files when not in portable mode)

### macOS
- **Settings Directory**: `~/Library/Deskflow/`
- **System-wide Directory**: `/Library/Deskflow/` (fallback for reading)

### Linux
- **Settings Directory**: `$XDG_CONFIG_HOME/Deskflow/` or `~/.config/Deskflow/`
- **State Directory**: `$XDG_STATE_HOME/` (for state files)
- **System-wide Directory**: `/etc/Deskflow/` (fallback for reading)

## Files and Directories Created

### 1. Configuration Files

#### Main Settings File
**File**: `Deskflow.conf` (INI format)
**Location**: 
- Windows: `%APPDATA%\Roaming\Deskflow\Deskflow.conf` or `<install-path>\settings\Deskflow.conf`
- macOS: `~/Library/Deskflow/Deskflow.conf`
- Linux: `~/.config/Deskflow/Deskflow.conf`

**Purpose**: Stores all GUI settings, including client/server configuration, network settings, and user preferences.
**Created**: Automatically on first launch if not found

#### Server Configuration File
**File**: `deskflow-server.conf` (Text format)
**Location**: Same directory as `Deskflow.conf`

**Purpose**: Stores server screen layout and configuration (which computers are connected and their positions)
**Created**: When running in server mode and saving the configuration

#### Application State File
**File**: `deskflow.state`
**Location**: 
- Linux: `$XDG_STATE_HOME/deskflow.state`
- Other platforms: Same directory as `Deskflow.conf`

**Purpose**: Stores application runtime state
**Created**: During application runtime

### 2. Log Files

#### User Log File
**File**: `Deskflow.log`
**Location**: User's home directory (`~/<AppId>.log`)

**Purpose**: General application logging
**Created**: When logging is enabled (configurable in settings)
**Rotation**: Rotates at 1MB file size

#### Daemon Log File (Windows)
**File**: `deskflow-daemon.log`
**Location**: Same directory as `Deskflow.conf`

**Purpose**: Daemon process logging on Windows
**Created**: When daemon is running
**Rotation**: Rotates at 1MB file size

#### Custom Log File
**File**: User-specified path
**Location**: Configurable via GUI settings

**Purpose**: Custom log output location
**Created**: When custom log path is configured
**Rotation**: Rotates at 1MB file size

### 3. TLS/Security Files

#### TLS Directory
**Directory**: `tls/`
**Location**: Subdirectory of settings directory (e.g., `~/.config/Deskflow/tls/`)

**Purpose**: Contains all TLS-related certificates and security files
**Created**: Automatically when TLS is enabled

#### SSL Certificate
**File**: `Deskflow.pem` (or `<AppId>.pem`)
**Location**: `<settings-directory>/tls/Deskflow.pem`

**Purpose**: Self-signed SSL/TLS certificate for encrypted connections
**Created**: Automatically generated on first use if not present
**Format**: PEM format (contains both certificate and private key)

#### Trusted Servers Database
**File**: `trusted-servers`
**Location**: `<settings-directory>/tls/trusted-servers`

**Purpose**: Stores fingerprints of trusted server certificates (for clients)
**Created**: When a client trusts a server's fingerprint
**Format**: Text file with fingerprint entries

#### Trusted Clients Database
**File**: `trusted-clients`
**Location**: `<settings-directory>/tls/trusted-clients`

**Purpose**: Stores fingerprints of trusted client certificates (for servers)
**Created**: When a server trusts a client's fingerprint
**Format**: Text file with fingerprint entries

### 4. Temporary and Test Files

#### Test Directories (Development Only)
**Directory**: `/tmp/test/`
**Location**: System temp directory

**Purpose**: Used by unit tests for testing file operations
**Created**: Only during test execution
**Cleaned up**: After tests complete

## File Creation Details

### Automatic Creation
Most files and directories are created automatically:

1. **Settings Directory**: Created on first launch if it doesn't exist
2. **TLS Directory**: Created when TLS is first enabled
3. **Configuration Files**: Created with default values on first use
4. **Log Files**: Created when logging is enabled
5. **Certificates**: Auto-generated if missing when TLS is enabled

### User-Triggered Creation
Some files are created by user actions:

1. **Server Configuration**: Saved when user configures screen layout
2. **Trusted Fingerprints**: Added when user accepts a certificate
3. **Custom Log Files**: Created at user-specified paths

### Platform-Specific Behavior

#### Windows
- Uses Windows Registry as fallback for settings
- Supports "portable mode" when `settings/Deskflow.conf` exists in install directory
- Service mode requires elevated permissions and uses ProgramData directory

#### macOS
- Requires accessibility permissions to create files in protected locations
- May use quarantine attributes on downloaded files

#### Linux
- Follows XDG Base Directory specification
- Respects `XDG_CONFIG_HOME` and `XDG_STATE_HOME` environment variables

## File Permissions

All created files and directories use default user permissions:
- Configuration files: User read/write only
- Log files: User read/write only
- TLS certificates and keys: User read/write only (sensitive data)

## Cleanup

To completely remove Deskflow data:

### Windows
1. Delete: `%APPDATA%\Roaming\Deskflow\`
2. Delete: `C:\ProgramData\Deskflow\` (if exists)
3. Delete: `<install-path>\settings\` (if in portable mode)
4. Remove registry keys: `HKCU\Software\Deskflow\`

### macOS
1. Delete: `~/Library/Deskflow/`
2. Delete: `/Library/Deskflow/` (if exists)

### Linux
1. Delete: `~/.config/Deskflow/`
2. Delete: `$XDG_STATE_HOME/deskflow.state`
3. Delete: `/etc/Deskflow/` (if exists, requires root)

## Diagnostic Clear Settings

The application provides a "Clear Settings" diagnostic function that:
1. Recursively removes the entire settings directory
2. Recreates an empty settings directory
3. For portable mode: Creates an empty `Deskflow.conf` file

**Warning**: This removes ALL settings, configurations, logs, and certificates.

## Summary Table

| File/Directory | Purpose | Auto-Created | Platform-Specific |
|:--------------|:--------|:------------|:-----------------|
| `Deskflow.conf` | Main settings | Yes | Path varies |
| `deskflow-server.conf` | Server config | On save | No |
| `deskflow.state` | Runtime state | Yes | Linux uses XDG |
| `*.log` | Log files | When enabled | Path varies |
| `tls/` directory | TLS files | When TLS enabled | No |
| `Deskflow.pem` | SSL certificate | Yes | No |
| `trusted-servers` | Server fingerprints | On trust | No |
| `trusted-clients` | Client fingerprints | On trust | No |

## Security Considerations

- **Certificates**: The `Deskflow.pem` file contains both the certificate and private key. Protect this file with appropriate permissions.
- **Fingerprints**: The trusted-servers and trusted-clients files prevent man-in-the-middle attacks. Don't modify these manually unless you understand the security implications.
- **Log Files**: May contain sensitive information. Configure log levels appropriately and secure log file locations.

## References

For more information about configuration options, see:
- [Configuration Documentation](configuration.md)
- Settings implementation: `src/lib/common/Settings.cpp`
- TLS utilities: `src/lib/gui/TlsUtility.cpp`
- Server config: `src/lib/gui/config/ServerConfig.cpp`
