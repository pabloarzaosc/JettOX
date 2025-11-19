# 🚀 JettOX

**Deep System Diagnostics Tool for Windows**

JettOX Pro is a comprehensive, automated diagnostics tool designed to collect detailed information about your system, hardware, firmware, storage, and connected peripherals. It is ideal for IT administrators, security auditors, and system enthusiasts who want a complete overview of a Windows machine.

---

## 🧩 Key Features

| Module                 | Description                                                                        | Estimated Time |
| ---------------------- | ---------------------------------------------------------------------------------- | -------------- |
| **System**             | Collects OS information, running processes, installed drivers, and event logs      | ~20s           |
| **Hardware**           | Gathers CPU, GPU, memory, motherboard, and logical/physical cores details          | ~15s           |
| **Firmware**           | Extracts BIOS/UEFI info, microcode updates, boot configuration, and power settings | ~15s           |
| **Storage**            | Collects disk info, partitions, usage, and SMART data if available                 | ~20s           |
| **Peripherals**        | Detects USB devices, network adapters, Bluetooth devices, and PnP devices          | ~15s           |
| **Drivers & Services** | Collects driver lists and service status for system troubleshooting                | ~10s           |

> ⏱ **Total estimated runtime:** ~95 seconds (varies by system)

---

## ⚡ Technologies Used

* **Python 3.10+** – Main language, cross-platform scripting
* [Rich](https://rich.readthedocs.io/) – Fancy terminal output with spinners, tables, and panels
* [psutil](https://pypi.org/project/psutil/) – CPU, memory, disk, and partition monitoring
* [pyfiglet](https://pypi.org/project/pyfiglet/) – Optional ASCII banners
* **PowerShell & WMIC** – Native Windows commands for low-level system queries
* **subprocess** – Safely executes external commands and captures outputs
* **shutil & os modules** – Directory management and filesystem handling
* JSON & CSV parsing for structured data output

---

## 📂 Folder Structure

```
/project-root
│
├─ firmware/        # BIOS/UEFI logs, microcode events, dxdiag
├─ hardware/        # CPU, GPU, memory, motherboard, logical cores
├─ peripherals/     # USB, Bluetooth, network adapters, PnP devices
├─ storage/         # Disks, partitions, SMART logs, usage stats
├─ system/          # Processes, drivers, Windows event logs
├─ logs/            # Execution logs (timestamps, warnings, errors)
└─ result/          # Aggregated JSON results and summaries
```

* Each folder contains a `summary.json` and `raw.txt` file with parsed and raw outputs.
* Logs include timestamped entries with INFO/WARN/ERROR levels.

---

## ⚡ How to Run

```bash
# Clone the repository
git clone https://github.com/pabloarzaoo/JettOX/

cd JettOX

# Install dependencies
pip install -r requirements.txt

# Run the diagnostics
python main.py
```

> 🔑 For full system information and firmware queries, run as **Administrator**.

**Optional Flags & Notes:**

* If `psutil` is not installed, hardware info will be limited.
* SMART disk info requires `smartctl` from Smartmontools.
* Admin privileges are required for full PowerShell and WMIC queries.

---

## 🖥️ Sample Output

```
✔ System finished in 18.34s
✔ Hardware finished in 14.21s
✔ Firmware finished in 15.12s
✔ Storage finished in 19.87s
✔ Peripherals finished in 14.45s
✔ Drivers & Services finished in 10.05s

Results saved to: /result/results_of_all_files.json
Logs saved to: /logs/run_YYYY-MM-DD_HHMMSS.txt
```

### Example JSON Output Snippet

```json
{
  "systeminfo_parsed": {
    "OS Name": "Microsoft Windows 10 Pro",
    "OS Version": "10.0.19045",
    "System Manufacturer": "Dell Inc.",
    "System Model": "XPS 15 9500"
  },
  "hardware": {
    "cpu": {"Name": "Intel(R) Core(TM) i7-10750H CPU @ 2.60GHz"},
    "memory_total_bytes": 17179869184,
    "logical_cpus": 12,
    "physical_cpus": 6
  }
}
```

---

## 🔍 Technical Details

JettOX Pro executes diagnostics in five main stages:

1. **Directories & Logging:**

   * Ensures all output folders exist (`firmware`, `hardware`, etc.).
   * Initializes a timestamped log file capturing INFO/WARN/ERROR messages.

2. **Module Execution:**

   * Runs Python functions for each module (`collect_system`, `collect_hardware`, etc.).
   * Uses `subprocess` to run PowerShell and WMIC commands safely.

3. **Parsing & Structuring:**

   * Converts raw command output into structured JSON.
   * Handles CSV parsing for processes and table-like outputs.
   * Includes error handling and fallback messages for missing tools.

4. **Results Aggregation:**

   * Combines all module outputs into `/result/results_of_all_files.json`.
   * Each module folder contains both raw output (`raw.txt`) and summary (`summary.json`).

5. **Terminal Visualization:**

   * Uses Rich library to display progress spinners, panels, and tables.
   * Optional ASCII banner using PyFiglet.
   * Displays elapsed time per module and overall runtime.

---

## 📊 Detailed Module Breakdown

### System Module

* `systeminfo`, `tasklist`, `driverquery` for Windows environment overview.
* Retrieves top 40 running processes as a sample.
* Collects Windows event logs (`System` and `Application`) for recent errors.

### Hardware Module

* CPU details: Name, cores, frequency (current, min, max).
* Memory details: total and available RAM.
* GPU info using WMIC.
* Optional: psutil provides logical/physical CPU counts.

### Firmware Module

* BIOS/UEFI: Manufacturer, version, release date, serial number.
* Power settings and boot configuration (`powercfg /q`, `bcdedit`).
* DXDiag outputs for DirectX and display info.
* Microcode events collected via PowerShell.

### Storage Module

* Disk drives, partitions, filesystem types, mountpoints.
* Disk usage statistics and optional SMART health checks via `smartctl`.

### Peripherals Module

* USB devices, Bluetooth devices, PnP devices, network adapters.
* Counts number of entries and logs raw outputs for reference.

### Drivers & Services Module

* Lists all installed drivers (`driverquery /v`) and services (`wmic service`).
* Captures errors or warnings if permissions are limited.

---

## ⚖️ License

MIT License – Free to use, modify, and share.

---

## 📌 Additional Notes & Tips

* Run multiple times to compare changes over time.
* Store `results_of_all_files.json` for auditing or troubleshooting.
* Use administrator privileges to maximize collected data.
* Optional tools:

  * `smartctl` for detailed disk health.
  * `psutil` for enhanced CPU/memory/disk info.
  * `pyfiglet` for a fancier terminal banner.





