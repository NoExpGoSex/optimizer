# Automating Optimizer with a JSON Template

Optimizer can be configured in a repeatable way by passing a JSON template file to the app.

## 1. Download a starter template

Use one of the template files that matches your Windows version:

- [Windows 7](https://github.com/hellzerg/optimizer/blob/master/templates/template-windows7.json)
- [Windows 8.1](https://github.com/hellzerg/optimizer/blob/master/templates/template-windows8.json)
- [Windows 10](https://github.com/hellzerg/optimizer/blob/master/templates/template-windows10.json)
- [Windows 11](https://github.com/hellzerg/optimizer/blob/master/templates/template-windows11.json)

## 2. Edit the template

Customize the JSON values to match your desired configuration.

## 3. Run Optimizer with the template

```bat
optimizer.exe /config=template.json
```

## Example template

```json
{
  "WindowsVersion": 11,
  "PostAction": {
    "Restart": false,
    "RestartType": "Normal"
  },
  "Cleaner": {
    "TempFiles": true,
    "BsodDumps": true,
    "ErrorReports": true,
    "RecycleBin": null,
    "InternetExplorer": null,
    "GoogleChrome": {
      "Cache": true,
      "Cookies": true,
      "History": true,
      "Session": true,
      "Passwords": null
    }
  },
  "Pinger": {
    "SetDNS": "Cloudflare",
    "CustomDNSv4": [],
    "CustomDNSv6": [],
    "FlushDNSCache": true
  },
  "ProcessControl": {
    "Prevent": ["chrome.exe", "firefox.exe"],
    "Allow": ["opera.exe"]
  },
  "HostsEditor": {
    "IncludeWwwCname": true,
    "Block": ["youtube.com", "google.com"],
    "Remove": ["facebook.com"],
    "Add": [
      {
        "Domain": "test.com",
        "IPAddress": "192.168.1.5"
      },
      {
        "Domain": "test2.com",
        "IPAddress": "192.168.1.9"
      }
    ]
  },
  "RegistryFix": {
    "TaskManager": null,
    "CommandPrompt": null,
    "ControlPanel": null,
    "FolderOptions": null,
    "RunDialog": null,
    "RightClickMenu": null,
    "WindowsFirewall": null,
    "RegistryEditor": null
  },
  "Integrator": {
    "TakeOwnership": false,
    "OpenWithCMD": false
  },
  "AdvancedTweaks": {
    "UnlockAllCores": null,
    "DisableHPET": null,
    "EnableRegistryBackups": null,
    "EnableLoginVerbose": null,
    "SvchostProcessSplitting": {
      "Disable": null,
      "RAM": null
    }
  },
  "Tweaks": {
    "DisableWindowsDefender": false,
    "DisableAutomaticUpdates": false,
    "EnableUtcTime": true,
    "DisableCoPilotAI": true
  }
}
```

## Template configuration reference

### WindowsVersion
- Required.
- Integer value matching the installed OS:
  - `7`
  - `8`
  - `10`
  - `11`

### Cleaner
- All options are nullable booleans.
- Use `true` to clean or remove the item.
- Use `false` to explicitly keep it.
- Use `null` to leave the option unchanged.

Example:

```json
"Cleaner": {
  "TempFiles": true,
  "BsodDumps": true,
  "RecycleBin": false,
  "GoogleChrome": {
    "Cache": true,
    "Cookies": true,
    "History": true
  }
}
```

### Pinger
- `SetDNS` chooses a predefined DNS provider.
- Supported values:
  - `Automatic`
  - `Custom`
  - `Cloudflare`
  - `OpenDNS`
  - `Quad9`
  - `Google`
  - `AlternateDNS`
  - `Adguard`
  - `CleanBrowsing`
  - `CleanBrowsing (adult filter)`
- `CustomDNSv4` and `CustomDNSv6` are arrays of custom DNS server IPs.
- `FlushDNSCache` is a boolean and should be set to `true` when you want the system DNS cache cleared.

Examples:

```json
"Pinger": {
  "SetDNS": "Cloudflare",
  "CustomDNSv4": [],
  "CustomDNSv6": [],
  "FlushDNSCache": true
}
```

```json
"Pinger": {
  "SetDNS": "Custom",
  "CustomDNSv4": ["3.3.3.3"],
  "CustomDNSv6": ["::1"],
  "FlushDNSCache": true
}
```

### ProcessControl
- Prevents or allows processes by filename.

```json
"ProcessControl": {
  "Prevent": ["chrome.exe", "firefox.exe"],
  "Allow": ["opera.exe"]
}
```

### HostsEditor
- `IncludeWwwCname`: when `true`, adds a `www.` entry for each domain you add or block.
- `Block`: list of domains to redirect to `0.0.0.0`.
- `Remove`: list of domains to remove from the HOSTS file.
- `Add`: list of custom records with `Domain` and `IPAddress`.

```json
"HostsEditor": {
  "IncludeWwwCname": true,
  "Block": ["youtube.com", "google.com"],
  "Remove": ["facebook.com"],
  "Add": [
    {
      "Domain": "test.com",
      "IPAddress": "192.168.1.5"
    },
    {
      "Domain": "test2.com",
      "IPAddress": "192.168.1.9"
    }
  ]
}
```

### RegistryFix
- Use boolean values to enable or reset specific registry-based fixes.

Example:

```json
"RegistryFix": {
  "TaskManager": true,
  "CommandPrompt": false,
  "WindowsFirewall": true
}
```

### Integrator
- `TakeOwnership`: adds or removes the "Take Ownership" item from the right-click menu.
- `OpenWithCMD`: adds or removes the "Open CMD here" item from the right-click menu.

```json
"Integrator": {
  "TakeOwnership": false,
  "OpenWithCMD": false
}
```

### AdvancedTweaks
- These are advanced and should only be used when you understand the impact.
- Common keys:
  - `DisableHPET`
  - `EnableLoginVerbose`
  - `UnlockAllCores`
  - `EnableRegistryBackups`

Example:

```json
"AdvancedTweaks": {
  "DisableHPET": false,
  "EnableLoginVerbose": true,
  "UnlockAllCores": null,
  "EnableRegistryBackups": true,
  "SvchostProcessSplitting": {
    "Disable": true,
    "RAM": 8
  }
}
```

### Tweaks
- This object contains many individual toggles.
- Use `true` to apply a tweak, `false` to reset it, and `null` to leave it unchanged.

Example:

```json
"Tweaks": {
  "DisableWindowsDefender": false,
  "DisableAutomaticUpdates": true,
  "EnableUtcTime": true,
  "DisableCoPilotAI": true
}
```

### PostAction
- Final action after the template is applied.

Available fields:

- `Restart`: boolean
- `RestartType`: one of:
  - `Normal`
  - `SafeMode`
  - `DisableDefender`
  - `EnableDefender`

Example:

```json
"PostAction": {
  "Restart": true,
  "RestartType": "Normal"
}
```

## Notes
- `null` and omitted values are treated as "do nothing" for most settings.
- `false` explicitly resets or disables a feature.
- Always review the template before running it, especially in `Tweaks` and `AdvancedTweaks`.

Important: this project is deprecated in favor of OptimizerNXT, but the config format above reflects the JSON schema used by the legacy Optimizer build.
