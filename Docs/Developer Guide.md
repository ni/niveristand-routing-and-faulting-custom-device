## Overview

## Producer/Consumer Design Pattern

The **Routing and Faulting Custom Device** and **SLSC Switch Custom Device** use a [producer/consumer design pattern](http://www.ni.com/tutorial/3023/en/).

The **Routing and Faulting Custom Device** is the producer, which monitors changes to VeriStand routing channels and sends disconnect/connect messages to the corresponding consumers.

Multiple **SLSC Switch Custom Devices** act as the consumers, which receive the messages and call into underlying hardware to disconnect/connect endpoints. Existing endpoints are disconnected before new endpoints are connected. Connections that remain unchanged do not glitch.

The following diagram illustrates the pattern for these devices.

![Switch Messages](Support\Switch%20Messages.png)

The producer/consumer design pattern is implemented in the [Custom Device Message Library](https://github.com/ni/niveristand-custom-device-message-library). The **Custom Device Message Library** uses named message queues to send requests and receive responses. The library defines base classes for the producer, consumer, and messages. It also defines derived classes for switch consumer and switch messages.

The [SLSC Switch Message Library](https://github.com/ni/niveristand-slsc-switch-message-library) overrides the switch consumer class and calls into the **SLSC Switch API** to connect and disconnect endpoints.

The following diagram illustrates the pattern for these libraries.

![Class Diagram](Support\Class%20Diagram.png)

## Adding New Hardware Support

The **Routing and Faulting Custom Device** supports **SLSC Switch** hardware. The Custom Device is designed to support additional hardware with minimal effort.

1. Override the abstract switch consumer class and implement the following hardware specific methods:

  | Method Name | Description |
  |---|---|
  | Initialize with Switch Parameters Core | Open a hardware session using the switch parameters. The switch parameters include the resource name and topology. |
  | Connect | Connect the specified endpoints. |
  | Disconnect | Disconnect the specified endpoints. |
  | Disconnect All | Disconnect all connected endpoints. |
  | Shutdown Core | Reset the hardware state and close the hardware session. |

2. Create a hardware specific custom device based on the asynchronous or inline asynchronous template.  

  _NOTE: Avoid the inline hardware template as it may adversely affect loop rates of the application._

3. Add the following properties to the system definition:

  | Property Name | Type | Description |
  |---|---|---|
  | SwitchVersion | Integer | Gets the read-only switch version, e.g. 1. The version determines which properties are supported. |
  | Model | String | Gets the read-only model name, e.g. "SLSC-12251". |
  | Resource | String | Gets the read-only hardware specific resource name, e.g. "slsc-12001-0314b282-Mod2". The resource name is compiled during deployment and sent to the engine. |
  | Topology | String | Gets and sets the hardware specific topology, e.g. "NI12251_topology". The topology is compiled during deployment and sent to the engine. Assign an empty string if the hardware does not support multiple topologies.|

4. Compile the required properties (and any hardware specific properties) during the OnCompile Action VI.

5. Initialize the hardware specific switch consumer in the engine and execute all incoming messages.

6. Shut down the consumer if an error occurs or if VeriStand sends the Shut Down message.

  The following image displays how the VI processes the message.

  ![RT Driver VI](Support\RT%20Driver%20VI.png)

## System Definition Compile

VeriStand compiles the system definition file before deploying the engine. The **Routing and Faulting Custom Device** compiles the routing channels into several lookup tables to optimize string manipulation in the engine.

The following diagram illustrates how these lookup tables are arranged.

![Compiled Routing and Faulting Configuration](Support\Compiled%20Routing%20and%20Faulting.png)

The list of connections consists of an array of state values and compiled connection indices:
* Each compiled connection index refers to an entry in the compiled connections.
* Each compiled connection contains a communication configuration index and connection string.
* Each communication configuration index refers to an entry in the communication configuration.

In the example below, the producer will perform and index lookup and send connect message to consumer queue name "Path/To/Switch" with a connection list of "A->B, C-D".

![Connection Lookup](Support\Connection%20Lookup.png)
