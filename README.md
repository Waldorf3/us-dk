# us-dk
The purpose of this is to enables € (Eurosign) and danish characters (æøå) on altGr for Linux with US keyboard. 

I made this explicitly for Fedora KDE 43, but it ought to work on most (all?) distros and GE.

# Custom US-DK Keyboard Setup

### 1. The Symbols File
**Path:** `/usr/share/X11/xkb/symbols/us-dk`

```text
default partial alphanumeric_keys
xkb_symbols "us-dk" {

    include "us(basic)"
    name[Group1]= "English (US, Danish AltGr symbols)";

    include "level3(ralt_switch)"

    // Mapping: [ Level 1, Level 2 (Shift), Level 3 (AltGr), Level 4 (Shift+AltGr) ]
    
    key <AE04> { [ 4,          dollar,      EuroSign,     EuroSign      ] };
    key <AD03> { [ e,          E,           EuroSign,     EuroSign      ] };
    key <AC10> { [ semicolon,  colon,       ae,           AE            ] };
    key <AC11> { [ apostrophe, quotedbl,    oslash,       Ooblique      ] };
    key <AD11> { [ bracketleft, braceleft,  aring,        Aring         ] };

};
```

### 2. The Registry Entry (XML)
**Path:** `/usr/share/X11/xkb/rules/evdev.xml`  
Find `<layout>` with `<name>us</name>` and add this to its `<variantList>`:

```xml
<variant>
  <configItem>
    <name>us-dk</name>
    <description>English (US, Danish AltGr symbols)</description>
  </configItem>
</variant>
```

### 3. Application Commands
Run these to force the system to bypass the cache and use the hyphenated file:

```bash
# Force system-wide map (Hyphens only)
sudo localectl set-x11-keymap us-dk pc105 us-dk

# Purge XKB binary caches (Mandatory for Wayland)
sudo rm -rf /var/lib/xkb/*
rm -rf ~/.cache/kbd
```
logout-login
---

### Logic Reference: The 4-Level Quadrant


| Key Position | Level 1 | Level 2 | Level 3 (R-Alt) | Level 4 (R-Alt+Shift) |
| :--- | :--- | :--- | :--- | :--- |
| `<AC10>` | `;` | `:` | **æ** | **Æ** |
| `<AC11>` | `'` | `"` | **ø** | **Ø** |
| `<AD11>` | `[` | `{` | **å** | **Å** |
| `<AE04>` | `4` | `$` | **€** | **€** |
