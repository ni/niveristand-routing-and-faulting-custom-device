## Overview

Many HIL test systems require hardware fault injection to simulate faults between the ECU and the rest of the system. The **Routing and Faulting Custom Device** allows users to inject faults using routing modules. The **SLSC Switch Custom Device** provides hardware support for the routing modules.

## Routing and Faulting

## SLSC Switch

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
