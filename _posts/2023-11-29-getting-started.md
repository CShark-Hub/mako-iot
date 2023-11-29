---
title: Getting started with MAKO IoT
date: 2023-11-29
categories: [How To..., get started]
tags: [how-to]     # TAG names should always be lowercase
author: Bart
comments: false
---
_see [MAKO IoT README](https://github.com/CShark-Hub/Mako-IoT.Home)_

## Pre-requisites
- Install [.NET nanoFramework](https://docs.nanoframework.net/content/getting-started-guides/index.html)

## Code your project
Create two [nanoFramework projects](https://docs.nanoframework.net/content/getting-started-guides/index.html). For example: _MyProject.Device_ (Class Library) and _MyProject.Device.App_ (nanoFramework Application).

**_MyProject.Device_**

Implement your logic here. You will be able to unit test this project (without hardware!). Place everything your software needs to do here, abstracting hardware-specific operations with interfaces. For example, if you want to blink a LED - create _IBlinker_ interface here with _void Blink(bool isOn);_ method.

**_MyProject.Device.App_**

This is the entry point to your software. Use _DeviceBuilder_ to link all components together (MAKO IoT and your code) in the _Main()_ method of _Program_ class. Also, implement your hardware-specific logic here. For example, concrete blinker class for the interface above: _LedBlinker : IBlinker_. Link the interface with the class in _ConfigureDI_ section of _DeviceBuilder_.

## Test your code
Create [nanoFramework test project](https://docs.nanoframework.net/content/unit-test/index.html) and implement unit tests for your logic from _MyProject.Device_ (Class Library).

## Give it a go!
Connect your device, Set Startup Project to _MyProject.Device.App_ (nanoFramework Application) and run it. 



Take a look at [Samples](https://github.com/CShark-Hub/Mako-IoT.Samples).

## MAKO IoT components

### Composition & Abstractions
- [Interface](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Interface) - Interfaces for most of MAKO IoT components. Start building your app here :)
- [Device](https://github.com/CShark-Hub/Mako-IoT.Device) - This brings all your MAKO IoT components together, configures your solution and runs your app.

### Configuration
- [Configuration](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Configuration) - Provides configuration capabilities to your solution. Along with [file storage component](https://github.com/CShark-Hub/Mako-IoT.Device.Services.FileStorage) reads & writes settings into devices's flash memory.

### External Configuration
With those componentes you can read & write configuration through REST API. This is particularly useful when setting up a device i.e. entering WiFi credentials.
- [Configuration Manager](https://github.com/CShark-Hub/Mako-IoT.Device.Services.ConfigurationManager) - Controls device's modes: normal operation or configuration mode. When device enters configuration mode, WiFi goes into Access Point mode and configuration API is served.
- [Configuration API](https://github.com/CShark-Hub/Mako-IoT.Device.Services.ConfigurationApi) - The REST API for configuring your device's settings.
- [Configration Metadata](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Configuration.Metadata) - Metadata provide a way to publish extra details about your settings, such as labels, data types etc. through the API. Decorate your configuration classes with attributes, build and run [Metadata Generator](https://github.com/CShark-Hub/Mako-IoT.Core.Configuration.MetadataGenerator).
- [Metadata Generator](https://github.com/CShark-Hub/Mako-IoT.Core.Configuration.MetadataGenerator) - This tool generates metadata from config attributes. Attributes are not fully supported in nanoFramework, so this can't be done on device.
- [Configuration App](https://github.com/CShark-Hub/Mako-IoT.Core.Configuration.App.Client) - Configuration API client with UI (Blazor WebAssembly app).
- [Configuration API Model](https://github.com/CShark-Hub/Mako-IoT.ConfigurationApi.Model) - Model for the configuration & metadata objects.

### Networking
- [WiFi](https://github.com/CShark-Hub/Mako-IoT.Device.Services.WiFi) - Provides basic WiFi network operations.
- [WiFi AP](https://github.com/CShark-Hub/Mako-IoT.Device.Services.WiFi.AP) - Handles WiFi in Access Point mode.
- [API/Web Server](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Server) - Simple on-device web server.

### Messaging
#### Message Bus - device
- [Message Bus](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Messaging) - Message bus with strongly-typed contracts and automatic (type & convention based) routing.
- [MQTT](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Mqtt) - MQTT transport for the [Message Bus](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Messaging).
- [AzureIoT](https://github.com/CShark-Hub/Mako-IoT.Device.Services.AzureIotHub) - AzureIoT transport for the [Message Bus](https://github.com/CShark-Hub/Mako-IoT.Device.Services.AzureIotHub).
#### Message Bus - .NET Core
- [Interfaces](https://github.com/CShark-Hub/Mako-IoT.Core.Services.Interface) - .NET Core interfaces for Message Bus
- [Message Bus](https://github.com/CShark-Hub/Mako-IoT.Core.Services.Messaging) - .NET Core implementation of Message Bus
- [MQTT](https://github.com/CShark-Hub/Mako-IoT.Core.Services.Mqtt) - MQTT transport for [Message Bus](https://github.com/CShark-Hub/Mako-IoT.Core.Services.Messaging)
#### Other
- [Data Providers](https://github.com/CShark-Hub/Mako-IoT.Device.Services.DataProviders) - If you need to periodically read a sensor or submit data of an event, this is where you start :)
- [ESP32 DeepSleep Data Provider](https://github.com/CShark-Hub/Mako-IoT.Device.Services.ESP32.DeepSleepDataProviders) - If you need to periodically read a sensor or submit data of an event and go to sleep, this is where you start :)
  
### In-process communication
- [Mediator](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Mediator) - Mediator pattern implementation. An easy-to-use publisher-subscriber bus.

### Storage
- [NVS File Storage](https://github.com/CShark-Hub/Mako-IoT.Device.Services.FileStorage) - Access built-in flash like a disk.

### System Utilities
- [Logging](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Logging) - Do your logging the [.NET way](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger?view=dotnet-plat-ext-7.0) on nanoFramework device!
- [Logs storage](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Logging.Storage) - Stores logs on internal storage and sends it to server.
- [Scheduler](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Scheduler) - Simple task scheduler - runs your code in defined periods of time.
- [Invoker](https://github.com/CShark-Hub/Mako-IoT.Device.Utilities.Invoker) - [Polly](https://github.com/App-vNext/Polly)-like retry mechanism for efficient [transient faults](https://learn.microsoft.com/en-us/azure/architecture/best-practices/transient-faults) handling.

### Hardware Specific
- [LED](https://github.com/CShark-Hub/Mako-IoT.Device.Displays.Led) - Blinks a LED in a multiple ways ("soft" PWM blinks, RBG color transitions etc.)

### Various Utilities
- [String](https://github.com/CShark-Hub/Mako-IoT.String) - Useful String operations.
- [Time Zones](https://github.com/CShark-Hub/Mako-IoT.TimeZones) - The way to get local time on your device instead just UTC :)
- [iCalendar](https://github.com/CShark-Hub/Mako-IoT.ICalParser) - Parses calendar events.

## [Samples](https://github.com/CShark-Hub/Mako-IoT.Samples)
- [Messaging](https://github.com/CShark-Hub/Mako-IoT.Device.Samples/tree/main/Messaging) - Two-way communication across device and .NET application through [Message Bus](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Messaging) and [MQTT](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Mqtt). Also demonstrates how to share code between nanoFramework and "big" .NET.
- [Configuration API](https://github.com/CShark-Hub/Mako-IoT.Samples/tree/main/ConfigurationAPI) - Shows how to configure your settings from external.
- [Log Storage](https://github.com/CShark-Hub/Mako-IoT.Samples/tree/main/LogStorage) - Example usage of logging to local storage and sending out the logs to Elasticsearch server.
- [Mediator](https://github.com/CShark-Hub/Mako-IoT.Samples/tree/main/Mediator) - Example of [Mediator](https://github.com/CShark-Hub/Mako-IoT.Device.Services.Mediator) usage.
- [Waste Bins Calendar](https://github.com/CShark-Hub/Mako-IoT.Samples/tree/main/WasteBinsCalendar) - Project of a device, which indicates bins for collection on a given day. It demonstrates how to compose multiple MAKO IoT building blocks into a useful product :)

