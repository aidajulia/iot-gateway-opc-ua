This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments

# OPC UA Client Module for the Azure IoT Gateway SDK release 2016-11-18
This reference implementation demonstrates how the Azure IoT Gateway SDK can be used to connect to existing OPC UA servers and send JSON encoded telemetry data from these servers via the soon to be released OPC UA "Pub/Sub" specification extension (part 14, currently in draft state) to Azure IoT Hub. All transports to Azure that the Gateway SDK supports can be used, i.e. HTTPS, AMQP and MQTT. The transport is selected in the Transport setting in gateway_config.JSON.

Visit https://github.com/OPCFoundation/UA-.NETStandardLibrary for the sources to the NuGet packages used in this implementation.

## Operating System Compatibility
Since this reference implementation is written for .NET, Windows 7, 8, 8.1 & 10 are supported right now. Once the Gateway SDK supports .NET Standard, so will this module and then Linux will also be supported.

## Directory Structure

### /binding
This folder contains the binding to the Gateway SDK, i.e. the native entry point required (main.c), the gateway configuration file (gateway_config.json) as well as the two OPC UA gateway module configuration files (Opc.Ua.Client.SampleModule.Config.xml and Opc.Ua.Client.SampleModule.Endpoints.xml).

## /inc
This folder contains the headers required for the dotnet binding project, copied from the Azure IoT SDK for convenience. **These should be replaced with the newest ones from the gateway SDK!**

### /lib
This folder contains the static libraries the native entry point needs to link with.

### Root Directory
This folder contains the C# OPC UA module source file (Module.cs) and the NuGet package configuration file (packages.config).

## Configuring the Sample Module
The OPC UA server endpoints the module should connect to as well as the list of OPC UA nodes for each endpoint that should be published to Azure IoT Hub can be configured in the **ListOfPublishedNodes** section of the **Opc.Ua.Client.SampleModule.Config.xml** configuration file. The **Current Server Time** node (node ID 2258) is specified as an example.

Also, in the gateway_config.json, configure the name of IoT Hub you want to send the telemetry to (JSON field "IoTHubName") as well as the IoT Hub device ID and shared access key to use (JSON field "args").

## Building and Running the Sample Module
To build the reference implementation, you first need to clone and build the Azure IoT Gateway SDK. See https://github.com/azure/azure-iot-gateway-sdk for more information and **follow the instructions for building the gateway with the "dotnet binding"**.

Once done, copy the just compiled **aziotsharedutil.lib** & .pdb, **gateway.lib** & .pdb, **nanomsg.lib** & .pdb and **parson.lib** & .pdb to the **/lib** folder. It is easiest just to search for them. If you find multiple copies, any of them will do.

Then copy the just compiled **dotnet.dll** & .pdb, **iothub.dll** & .pdb, **Microsoft.Azure.IoT.Gateway.dll** & .pdb and **nanomsg.dll** & .pdb to the **/binding** folder. Again, just to search for them and if you find multiple copies, any of them will do).

Then open the solution file (**Opc.Ua.Client.Module.sln**) in Visual Studio and rebuild the solution. This will first restore the NuGet packages and then build both the binding project and the module project.

Then run the binding sample in the debugger. Enjoy!
