# Routing and Faulting Custom Device

The **Routing and Faulting Custom Device** allows users to configure routes and inject faults using SLSC Switch and Fault Insertion modules.

## Using the Custom Device

- Download the latest release package from the [Releases page](https://github.com/ni/niveristand-routing-and-faulting-custom-device/releases/).
- See the [User Guide](Docs/User%20Guide.md) for a walkthrough of using the Custom Device.
- See the [Developer Guide](Docs/Developer%20Guide.md) for information about extending the Custom Device.

## LabVIEW Source Code Version

LabVIEW 2019

## Dependencies

### Running the custom device

- [VeriStand 2019 or later](https://www.ni.com/en-us/support/downloads/software-products/download.veristand.html)
  - Includes [NI-SLSC](https://www.ni.com/en-us/support/downloads/drivers/download.ni-slsc.html)
- [NI-SWITCH](https://www.ni.com/en-us/support/downloads/drivers/download.ni-switch.html)

### Real-Time target software components

- NI-SWITCH (if using the NI-SWITCH Custom Device)
- NI-SLSC Switch (if using the NI-SLSC Switch Custom Device)

### Developing or building from source

- [LabVIEW 2019 or later](https://www.ni.com/en-us/support/downloads/software-products/download.labview.html)
- [LabVIEW Real-Time Module](https://www.ni.com/en-us/support/downloads/software-products/download.labview-real-time-module.html)
- `CDMessaging.lvlibp` from [Custom Device Message Library](https://github.com/ni/niveristand-custom-device-message-library)
- `SLSCSwitchMessaging.lvlibp` from [Routing and Faulting Message Library](https://github.com/ni/niveristand-routing-and-faulting-message-library)
- `NISWITCHMessaging.lvlibp` from [Routing and Faulting Message Library](https://github.com/ni/niveristand-routing-and-faulting-message-library)
- [VeriStand Custom Device Development Tools](https://github.com/ni/niveristand-custom-device-development-tools)
  - Install the latest package from the [release page](https://github.com/ni/niveristand-custom-device-development-tools/releases)
- [VeriStand Custom Device Testing Tools](https://github.com/ni/niveristand-custom-device-testing-tools)
  - Install the latest package from the [release page](https://github.com/ni/niveristand-custom-device-testing-tools/releases)

## Git History & Rebasing Policy

Branch rebasing and other history modifications will be listed here, with several notable exceptions:
- Branches prefixed with `dev/` may be rebased, overwritten, or deleted at any time.
- Pull requests may be squashed on merge.

## License

The Routing and Faulting Custom Device is licensed under an MIT-style license (see LICENSE). Other incorporated projects may be licensed under different licenses. All licenses allow for non-commercial and commercial use.
