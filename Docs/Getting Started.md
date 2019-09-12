## Overview

Many HIL test systems require hardware fault injection to simulate faults between the ECU and the rest of the system. The **Routing and Faulting Custom Device** enables users to inject faults using routing modules. The **SLSC Switch Custom Device** provides hardware support for the SLSC Switch routing modules.

The routing module acts as a gate between the ECU and the I/O. A common configuration in a fault injection application is a "fault bus topology" where you short each channel to one or more buses.

| States | Description | Diagram |
|---|---|---|
| Passthrough | The ECU and the I/O are connected without fault. This is the default mode of operation. | ![Passthrough](https://ni.scene7.com/is/image/ni/wraiiira5591242785034529104?scl=1) |
| Open Circuit | The ECU and the I/O are not connected simulating an open circuit. | ![Open Circuit](https://ni.scene7.com/is/image/ni/lrknfeim3638940521012328059?scl=1)|
| Short to Ground | The ECU and the system ground are connected simulating a short to ground. | ![Short to Ground](https://ni.scene7.com/is/image/ni/yijyimpy803861224648293470?scl=1) |
| Short to Power | The ECU and the system power are connected simulating a short to power. | ![Short to Power](https://ni.scene7.com/is/image/ni/yijyimpy803861224648293470?scl=1) |
| Pin-to-Pin Short | The ECU is connected to itself simulating a pin-to-pin short.  | ![Pin-to-pin short](https://ni.scene7.com/is/image/ni/bbkmcugz4974244261650505563?scl=1) |


## Routing and Faulting Custom Device

## SLSC Switch Custom Device

## Scripting API

## (EAR) Workflow

## Glossary

**alias**   An alias defines an alternate name for an endpoint for improved readability. A user typically aliases endpoints to match their DUT pins.

**connection**   A connection consists of a source and destination endpoint on a routing module. The routing module closes one or more switches to establish a connection.

**ECU**   Electronic control unit.

**endpoint**   An endpoint defines a source or destination pin on a routing module. A user typically connects their DUT pins to endpoints.

**HIL**   Hardware in the loop.

**routing channel**   A VeriStand routing channel consists of one or more potential states and a current state. The current state determines which connections are active. A user can set the current state by binding a ring control to a VeriStand routing channel.

**state**   Each state consists of zero or more connections. The Scripting API creates a single state with no connections for the initial state by default. A user can add additional states to a routing channel.

## References

[Using Fault Insertion Units (FIUs) for Electronic Testing](http://www.ni.com/en-us/innovations/white-papers/09/using-fault-insertion-units--fius--for-electronic-testing.html)

[Save Time and Maximize Reuse in HIL Testing with the SLSC Extension for PXI and CompactRIO](https://www.ni.com/en-us/innovations/white-papers/18/save-time-and-maximize-reuse-in-hil-testing-with-the-slsc-extens.html)