# ResolveOS Calamares Configuration

<p align="center">
  <strong>Calamares installer configuration and branding for ResolveOS</strong>
</p>

## About

This repository contains the Calamares installer configuration and branding files for ResolveOS. Calamares is a modern, flexible, and user-friendly installer framework used to install the operating system.

## What is Calamares?

Calamares is an installer framework that provides:
- Modern, Qt-based graphical interface
- Modular architecture
- Support for multiple bootloaders (GRUB, systemd-boot, rEFInd)
- Customizable branding and themes
- Multiple partitioning options
- Network installation capabilities

## Repository Structure

The repository contains:
- **calamares/**: Main configuration directory
  - **settings*.conf**: Multiple configuration profiles (standard, beginner, advanced, advanced-no-nvidia)
  - **branding/resolveosnew/**: Branding configuration, images (welcome screen, slideshow, logos), and stylesheets
  - **modules/**: Individual module configurations for bootloader, display manager, filesystem, locale, and network installation options

## Branding Configuration

### Current ResolveOS Branding

**File**: `calamares/branding/resolveosnew/branding.desc`

```yaml
componentName: resolveosnew

strings:
    productName:         resolveos
    shortProductName:    resolveos
    version:             1.0
    shortVersion:        1.0
    versionedName:       resolveos
    shortVersionedName:  resolveos
    bootloaderEntryName: resolveos

windowSize: 1200px,800px
windowExpanding: normal
```

### Branding Assets

#### Images Required

1. **Welcome Screen**: `calamares-0.png` (1366x768px)
   - Displayed on the welcome page
   - Should introduce ResolveOS

2. **Slideshow Images**: `calamares-1.png` through `calamares-13.png` (1366x768px)
   - Shown during installation
   - Current slides cover:
     - System features
     - Desktop environment
     - Applications
     - Tips and tricks

3. **Sidebar Logo**: `squid.png` (80x80px)
   - Small logo in the installer sidebar
   - Represents installation steps

4. **Window Icon**: `logo.png` (128x128px)
   - Window title bar icon
   - System tray representation

#### Stylesheets

- **stylesheet.qss**: Qt stylesheet for custom styling
- **stylesheet-codes.qss**: Color scheme definitions

## Settings Configurations

### 1. settings.conf (Standard)

Default configuration with:
- Full module selection
- GRUB bootloader
- Standard partitioning
- Network installation options

### 2. settings-beginner.conf

Simplified configuration for new users:
- Reduced options
- Automated choices
- Basic partitioning
- Essential modules only

### 3. settings-advanced.conf

Full-featured configuration:
- All available modules
- Advanced partitioning
- NVIDIA driver support
- Multiple bootloader options
- Network installation modules

### 4. settings-advanced-no-nvidia.conf

Same as advanced but without NVIDIA-specific modules.

## Module Configuration

### Core Modules

- **bootloader**: Installs and configures bootloader
- **displaymanager**: Sets up display/login manager
- **fstab**: Generates filesystem table
- **grubcfg**: Configures GRUB
- **initcpio**: Generates initial ramdisk
- **locale**: Sets system locale
- **mount**: Handles partition mounting

### Network Installation Modules

Allow selecting additional software during installation:

- `netinstall-applications.yaml` - General applications
- `netinstall-desktop.yaml` - Desktop environments
- `netinstall-desktop-wayland.yaml` - Wayland compositors
- `netinstall-drivers.yaml` - Hardware drivers
- `netinstall-nvidia.yaml` - NVIDIA drivers
- `netinstall-gaming.yaml` - Gaming software
- `netinstall-graphics.yaml` - Graphics applications
- `netinstall-multimedia.yaml` - Media players and editors
- `netinstall-development.yaml` - Development tools
- `netinstall-office.yaml` - Office suites
- And more...

### Bootloader Options

ResolveOS Calamares supports three bootloaders:

1. **GRUB** (default)
   - Traditional bootloader
   - Wide hardware support
   - Configured via `bootloader-grub.conf`

2. **systemd-boot**
   - Modern UEFI bootloader
   - Simpler configuration
   - Configured via `bootloader-systemd.conf`

3. **rEFInd**
   - Graphical boot manager
   - Multi-OS support
   - Configured via `bootloader-refind.conf`

## Integration with ISO

The Calamares configuration must be copied into the ISO build:

```bash
cd ~/resolveos-work
sudo mkdir -p resolveos-iso/archiso/airootfs/etc/calamares
sudo cp -r resolveos-calamares-config/calamares/* \
  resolveos-iso/archiso/airootfs/etc/calamares/
```

This is typically done as part of the ISO build process.

## Customizing Branding

### Changing Text

Edit `calamares/branding/resolveosnew/branding.desc`:

```yaml
strings:
    productName:         YourDistroName
    shortProductName:    YourDistro
    version:             1.0
    productUrl:          https://your-website.com/
    supportUrl:          https://your-website.com/support
```

### Replacing Images

1. **Create your images** at the correct sizes:
   - Welcome + Slideshow: 1366x768px
   - Sidebar logo: 80x80px
   - Window icon: 128x128px

2. **Replace the files**:
   ```bash
   cd calamares/branding/resolveosnew/
   cp /path/to/your/welcome.png calamares-0.png
   cp /path/to/your/slide1.png calamares-1.png
   # ... continue for all slides
   cp /path/to/your/logo.png logo.png
   cp /path/to/your/icon.png squid.png
   ```

### Customizing Colors

Edit `calamares/branding/resolveosnew/stylesheet.qss` to change colors, fonts, and styling.

## Testing Changes

After modifying configurations:

1. **Copy to ISO build**:
   ```bash
   sudo cp -r calamares/* \
     ../resolveos-iso/archiso/airootfs/etc/calamares/
   ```

2. **Rebuild ISO**:
   ```bash
   cd ../resolveos-iso/archiso
   ./build-iso.sh
   ```

3. **Test in VM**:
   - Boot the ISO in VirtualBox or QEMU
   - Launch Calamares
   - Verify branding and functionality

## Configuration Files Reference

### Module Configuration Format

Each module has a `.conf` file:

```yaml
---
# Module-specific settings
key: value
another_key: another_value
```

### Settings Configuration Format

Main settings files follow this structure:

```yaml
---
modules-search: [ local, /etc/calamares/modules ]

instances:
- id: module_instance
  module: module_name
  config: module-config.conf

sequence:
- show:
  - welcome
  - locale
  - ...
- exec:
  - partition
  - mount
  - ...
- show:
  - finished
```

## Updating Calamares

Calamares is included in the ISO's package list. To update:

1. Update `resolveos-iso/archiso/packages.x86_64`:
   ```
   calamares
   ```

2. Ensure compatibility with your configuration version

3. Test thoroughly after updates

## Troubleshooting

### Calamares Fails to Start

**Check**:
- Configuration files are in `/etc/calamares/`
- `calamares` package is installed
- Module files are present

**Debug**:
```bash
calamares -d  # Run with debug output
```

### Branding Not Showing

**Check**:
- Image files exist in branding directory
- File paths in `branding.desc` are correct
- Images are valid PNG files

### Module Errors

**Check**:
- Module configuration files exist
- Module IDs match in settings files
- Required system tools are installed

## Related Repository

- **[resolveos-iso](../resolveos-iso/)**: ResolveOS ISO builder

## Resources

- **Calamares Documentation**: https://github.com/calamares/calamares/wiki
- **Calamares Configuration**: https://github.com/calamares/calamares/wiki/Deploy-Configuration
- **Module Development**: https://github.com/calamares/calamares/wiki/Develop-Guide

## License

See individual files for licensing information. Based on ArcoLinux configurations which are GPL-licensed.

---

**Last Updated**: November 20, 2025  
**Version**: 1.0  
**Compatible with**: Calamares 3.x
