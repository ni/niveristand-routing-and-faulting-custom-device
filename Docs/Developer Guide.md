## Overview

## Producer/Consumer Design Pattern

The **Routing and Faulting Custom Device** and **SLSC Switch Custom Device** use a [producer/consumer design pattern](http://www.ni.com/tutorial/3023/en/). The **Routing and Faulting Custom Device** is the producer sending messages to multiple **SLSC Switch Custom Devices** acting as the consumers. The producer monitors changes to VeriStand routing channels and sends disconnect/connect messages to the corresponding consumers. The consumers receive the messages and call into underlying hardware to disconnect/connect endpoints. Existing endpoints are disconnected before new endpoints are connected. Connections that remain unchanged do not glitch.

![Switch Messages](Switch%20Messages.png)

The producer/consumer design pattern is implemented in the [Custom Device Message Library](https://github.com/ni/niveristand-custom-device-message-library). The **Custom Device Message Library** uses named message queues to send requests and receive responses. The **Custom Device Message Library** defines base classes for the producer, consumer, and messages. It also defines derived classes for switch consumer and switch messages. The [SLSC Switch Message Library](https://github.com/ni/niveristand-slsc-switch-message-library) overrides the switch consumer classes and calls into the **SLSC Switch API** to connect and disconnect endpoints.

## Adding New Hardware Support

## System Definition Compile