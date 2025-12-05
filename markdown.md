# Determine and Configure Hardware Settings
</table> Modern hardware requires proper configuration before and after installing an operating system. In this guide, you’ll learn how to configure firmware settings (BIOS/UEFI), detect hardware in Linux, inspect devices on PCI and USB buses, manage kernel modules, and explore kernel information in /proc and /sys.

  1. Firmware Configuration: BIOS vs UEFI
Computers use firmware interfaces to initialize hardware and start the boot process. Legacy systems rely on BIOS (Basic Input/Output System), while most modern machines use UEFI (Unified Extensible Firmware Interface).

Feature
</table> Initialization	Legacy MBR boot	GPT support, faster boot
Configuration interface	Text-based menu	Graphical menu, mouse support
Firmware updates	Manufacturer-specific flasher tools	Built-in update utilities
Hardware testing & config	Basic POST (Power-On Self-Test)	Extended diagnostics, secure boot, variable storage
