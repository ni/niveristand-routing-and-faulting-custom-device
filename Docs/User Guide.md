## Overview

Many HIL test systems require a hardware fault injection to simulate faults between the DUT and the rest of the system. The **Routing and Faulting Custom Device** injects faults by using switching and fault insertion modules. The **SLSC Switch Custom Device** provides hardware support for SLSC Switch routing modules.

The routing module acts as a gatekeeper between the DUT and the I/O. A common configuration in a fault injection application is a "fault bus topology." In this topology, endpoints are shorted to one or more buses.

The following table displays common topology states.

| States | Description | Diagram |
|---|---|---|
| Passthrough | The DUT and the I/O are connected without fault. This is the default state. | ![Passthrough](https://ni.scene7.com/is/image/ni/wraiiira5591242785034529104?scl=1) |
| Open Circuit | The DUT and the I/O are not connected, simulating an open circuit. | ![Open Circuit](https://ni.scene7.com/is/image/ni/lrknfeim3638940521012328059?scl=1)|
| Short to Ground | The DUT and the system ground are connected, simulating a short to ground. | ![Short to Ground](https://ni.scene7.com/is/image/ni/yijyimpy803861224648293470?scl=1) |
| Short to Power | The DUT and the system power are connected, simulating a short to power. | ![Short to Power](https://ni.scene7.com/is/image/ni/yijyimpy803861224648293470?scl=1) |
| Pin-to-Pin Short | Two or more DUT pins are connected, simulating a pin-to-pin short. | ![Pin-to-pin short](https://ni.scene7.com/is/image/ni/bbkmcugz4974244261650505563?scl=1) |

## SLSC Switch Custom Device

The **SLSC Switch Custom Device** is a VeriStand add-on that supports SLSC routing modules. The following modules are supported:
* [SLSC-12251](https://www.ni.com/en-us/support/model.slsc-12251.html)
* [SLSC-12252](https://www.ni.com/en-us/support/model.slsc-12252.html)
* [SET-2010](https://www.smart-e-tech.de/en/products/signal-conditioning-with-slsc/set-slsc-cards/set-2010-routing-card/)

SLSC Switch routing modules are displayed in the VeriStand System Explorer under _Targets >> Controller >> Hardware >> SLSC >> SLSC Chassis >> Modules >> Slot (n)_.

The following image displays this location.

![SLSC Switch System Explorer](Support/SLSC%20Switch%20System%20Explorer.png)

The **SLSC Switch Custom Device** enumerates all available endpoints for a routing module. Set the configuration for each endpoint to inform the SLSC Switch driver of valid endpoints to connect to.

The following table describes the available endpoint configurations.

| Configuration | Description |
|---|---|
| Load | A load endpoint is unrestricted and can be connected to other load and source endpoints. This is the default configuration for an endpoint. |
| Source | A source endpoint is restricted and cannot be connected to another source endpoint. |
| Configuration | A configuration endpoint is reserved by the hardware for internal routing. This configuration cannot be used directly. |

_NOTE: Configure the SLSC Switch Custom Device before configuring the Routing and Faulting Custom Device._

### Reservation Group

The SLSC Switch Custom Device initializes its SLSC Switch session with a custom _reservation group_. This initialization allows SLSC sessions with the same group to concurrently reserve the device. Avoid modifying SLSC properties used by SLSC Switch through these external sessions, as this will corrupt the switching state and establish unintended connections.

Use SLSC Switch 20.0.1 or newer to apply the reservation group. If you use SLSC Switch 19.x or 20.0.0, the custom device will silently ignore the reservation group.

By default the reservation group for the SLSC Switch Custom Device is the path to the module in the system definition file. Use the scripting API to update this value.

## Routing and Faulting Custom Device

The **Routing and Faulting Custom Device** is a VeriStand add-on that configures aliases and routing channels. By default, routing channels are not added. Add routing channels for each controllable collection of connections.

Routing Channels are displayed in the VeriStand System Explorer under _Targets >> Controller >> Custom Devices >> Routing and Faulting >> Routing Channels_.

The following image displays this location.

![Routing and Faulting System Explorer](Support/Routing%20and%20Faulting%20System%20Explorer.png)

Each routing channel contains one or more user-defined states. Common states for a fault injection application include _Passthrough_, _Open Circuit_, _Short to Ground_, _Short to Power_, and _Pin-to-Pin Short_. By default, the _Open Circuit_ state is added. Add additional states to match the requirements of the fault injection application.

Each state contains a collection of connections. Each connection consists of a source and destination endpoint on a routing module. By default, no connections are added. Add connections to establish connections between the DUT and the I/O. A single connection cannot span multiple routing modules. Define multiple connections, one for each routing module, to connect a signal spanning multiple routing modules.

Map a ring control on a VeriStand screen to a routing channel to set the current state. Set the current state to change the active connections between the DUT and the I/O. Existing connections from the previous state are disconnected before new connections from the current state are connected. Connections that remain unchanged from the previous state to the current state are not modified.

The following image displays how to configure a ring control on the VeriStand Editor.

![UI Manager](Support/UI%20Manager.png)

## Scripting API

The **Scripting API** uses LabVIEW VIs to configure modules, endpoints, aliases, and routing channels in a VeriStand system definition file.

_Note: The **SLSC Switch Custom Device** must be assigned to a slot in the **SLSC Chassis Custom Device** before running the example. The **SLSC Chassis Custom Device** is displayed in the VeriStand System Explorer under Targets >> Controller >> Hardware >> SLSC >> SLSC Chassis._

Open the scripting examples on disk from `Source/Scripting Examples/Scripting Examples.lvproj`. Run `Configure Modules from Hardware.vi` to configure one or more SLSC Switch routing modules in the system definition file.

The following image displays the `Configure Modules from Hardware.vi`.

![Configure Modules from Hardware](Support/Configure%20Modules%20From%20Hardware.png)

Run `Configure Routing and Faulting Custom Device.vi` to add routing channels, states, and connections. The example adds two routing channels named _Window Up_ and _Window Down_. Both routing channels have possible states of _No Connections_, _Passthrough_, _Short to Ground_, _Short to Power_, and _Open Circuit_. The _Window Up_ routing channel connects the _DUT_Ch0_ endpoint to the fault buses. The _Window Down_ routing channel connects the _DUT_Ch1_ endpoint to the fault buses.

The following image displays the `Configure Routing and Faulting Custom Device.vi`.

![Configure Routing and Faulting Custom Device](Support/Configure%20Routing%20and%20Faulting%20Custom%20Device.png)

Open the scripted system definition file in VeriStand to review the configuration or deploy the project.

## Glossary

**alias**   An alias is an alternate name for an endpoint for improved readability. Alias endpoints to match application DUT pins or I/O.

**connection**   A connection consists of a source and destination endpoint on a routing module. The routing module closes one or more switches to establish a connection. The routing module opens one or more switches to break a connection.

**DUT**   The device under test (DUT) is the hardware being tested for faults. The DUT may be an electronic control unit (ECU) or a line-replaceable unit (LRU).

**endpoint**   An endpoint is a source or destination pin on a routing module. A typical application connects the DUT pins or I/O to endpoints. In NI Switch or NI SLSC Switch this is also known as a channel.

**HIL**   Hardware in the loop.

**I/O**   Input/output.

**routing channel**   A VeriStand routing channel consists of one or more potential states and a current state. The current state determines which connections are active.

**state**   Each state consists of zero or more connections. The Scripting API creates a single state with no connections for the initial state by default.

## References

[Using Fault Insertion Units (FIUs) for Electronic Testing](http://www.ni.com/en-us/innovations/white-papers/09/using-fault-insertion-units--fius--for-electronic-testing.html)

[Save Time and Maximize Reuse in HIL Testing with the SLSC Extension for PXI and CompactRIO](https://www.ni.com/en-us/innovations/white-papers/18/save-time-and-maximize-reuse-in-hil-testing-with-the-slsc-extens.html)

[Using the SLSC-12251/2 Fault Insertion and Current Sensing Unit](https://www.ni.com/en-us/innovations/white-papers/19/using-the-slsc-12251-2-fault-insertion-and-current-sensing-unit.html)
