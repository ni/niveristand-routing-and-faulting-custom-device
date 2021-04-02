## Overview

## Producer/Consumer Design Pattern

The **Routing and Faulting Custom Device** and **SLSC Switch Custom Device** use a [producer/consumer design pattern](http://www.ni.com/tutorial/3023/en/).

The **Routing and Faulting Custom Device** is the producer, which monitors changes to VeriStand routing channels and sends disconnect/connect messages to the corresponding consumers.

Multiple **SLSC Switch Custom Devices** act as the consumers, which receive the messages and call into underlying hardware to disconnect/connect endpoints. Existing endpoints are disconnected before new endpoints are connected. Connections that remain unchanged do not glitch.

The following diagram illustrates the pattern for these devices.

![Switch Messages](Support/Switch%20Messages.png)

The producer/consumer design pattern is implemented in the [Custom Device Message Library](https://github.com/ni/niveristand-custom-device-message-library). The **Custom Device Message Library** uses named message queues to send requests and receive responses. The library defines base classes for the producer, consumer, and messages. It also defines derived classes for switch consumer and switch messages.

The [Routing and Faulting Message Library](https://github.com/ni/niveristand-routing-and-faulting-message-library) provides implementations which override the switch consumer class and call into device drivers to connect and disconnect endpoints.

The following diagram illustrates the pattern for these libraries.

![Class Diagram](Support/Class%20Diagram.png)

## Extending Hardware Support

### Supporting New Driver Types

As seen above, support for new driver types can be added by extending the Switch Consumer class. Switch Consumer implementations for **SLSC Switch** and **NI-SWITCH** hardware are available in the [Routing and Faulting Message Library](https://github.com/ni/niveristand-routing-and-faulting-message-library).

New versions of this library can be created to add support for new driver types by following the [template instructions](https://github.com/ni/niveristand-routing-and-faulting-message-library/tree/main/Source/template-instructions.md) in the Source Directory of the [Routing and Faulting Message Library](https://github.com/ni/niveristand-routing-and-faulting-message-library) repo.

Once a new Messaging Library is created, the following VIs need to be implemented for your given driver type.

#### Initialize with Switch Parameters Core

![Initialize with Switch Paramters Core](Support/Consumer%20Methods/Initialize%20with%20Switch%20Parameters%20Core.png)

Open a hardware session using the given instance of the Switch Parameters class. The parameters include the resource name, topology, and any custom properties for your device.

#### Connect

![Connect](Support/Consumer%20Methods/Connect.png)

Connect the specified endpoints. Connection list will be a string that follows the [Connection and Disconnection List Syntax](https://zone.ni.com/reference/en-XX/help/375472H-01/switch/connection_disconnection_list/).

#### Disconnect

![Disconnect](Support/Consumer%20Methods/Disconnect.png)

Disconnect the specified endpoints. Disconnection list will be a string that follows the [Connection and Disconnection List Syntax](https://zone.ni.com/reference/en-XX/help/375472H-01/switch/connection_disconnection_list/).

#### Disconnect All

![Disconnect All](Support/Consumer%20Methods/Disconnect%20All.png)

Disconnect all connected endpoints.

#### Shutdown Core

![Shutdown Core](Support/Consumer%20Methods/Shutdown%20Core.png)

Reset the hardware state and close the hardware session.

### Creating a New Switch Consumer Custom Device

Once a Switch Consumer exists for your given driver, follow these directions to createa a Custom Device that configures and executes your Switch Consumer for your specific device.

1. If no Custom Device exists for your device, create a new custom device based on the asynchronous or inline asynchronous template.

  _NOTE: Avoid the inline hardware template as it may adversely affect loop rates of the application._

2. Use the `Routing and Faulting Hardware API.lvlib::Set API Version.vi` in your Custom Device's Initialization VI to set the Routing and Faulting Hardware API version and opt in to being discovered by the Routing and Faulting Custom Device. The version number determines which properties are supported. Currently the only version is `1`.

3. Use the `Routing and Faulting Hardware API.lvlib::Set All Possible Connections.vi` in your Custom Device's Initialization VI to enumerate the possible direct connections for each endpoint in your device. For devices where user configuration affects what endpoints are connectable, you will also need to set this property any time a relevant configuration change occurs.

4. Compile the required properties (and any hardware specific properties) during the OnCompile Action VI.

5. Initialize the switch consumer for your hardware in the engine and execute all incoming messages.

6. Shut down the consumer if an error occurs or if VeriStand sends the Shut Down message.

  _NOTE: The following image shows the recommended pattern for processing messages in the engine of your Custom Device._

  ![The recommended pattern for processing messages in the engine of your Custom Device](Support/RT%20Driver%20VI.png)

## System Definition Compile

VeriStand compiles the system definition file before deploying the engine. The **Routing and Faulting Custom Device** compiles the routing channels into several lookup tables to optimize string manipulation in the engine.

The following diagram illustrates how these lookup tables are arranged.

![Compiled Routing and Faulting Configuration](Support/Compiled%20Routing%20and%20Faulting.png)

The list of connections consists of an array of state values and compiled connection indices:
* Each compiled connection index refers to an entry in the compiled connections.
* Each compiled connection contains a communication configuration index and connection string.
* Each communication configuration index refers to an entry in the communication configuration.

In the example below, the producer will perform and index lookup and send connect message to consumer queue name "Path/To/Switch" with a connection list of "A->B, C-D".

![Connection Lookup](Support/Connection%20Lookup.png)