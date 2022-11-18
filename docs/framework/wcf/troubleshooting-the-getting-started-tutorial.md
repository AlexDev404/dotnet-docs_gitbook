---
description: "Learn more about: Troubleshoot the Get started with Windows Communication Foundation tutorials"
title: Troubleshoot the Get started with Windows Communication Foundation tutorials
ms.date: 01/25/2019
ms.assetid: 69a21511-0871-4c41-9a53-93110e84d7fd
---
# Troubleshoot the Get started with Windows Communication Foundation tutorials

This article provides solutions for the most common problems and errors you might face when you follow the steps in the [Tutorial: Get started with Windows Communication Foundation applications](getting-started-tutorial.md).
  
## Common problems

**I can't find the project files on my hard drive.**

 Visual Studio saves project files in *C:\Users\\&lt;user name&gt;\source\repos*.  

**I can't find the *App.config* file generated by *Svcutil.exe*.**

 In Visual Studio, the **Add Existing Item** window only displays files with the following extensions by default:

- *.cs*
- *.resx*
- *.settings*
- *.xsd*
- *.wsdl*

To display all file types, select **All Files (\*.\*)** in the drop-down list in the lower-right corner of the **Add Existing Item** window.  
  
## Common errors

### Compile the service application

**Error BC30420 'Sub Main' was not found in 'GettingStartedHost.Module1'.**

The entry point is incorrect for the Visual Basic application. Make the following change:

   1. In the **Solution Explorer** window, select the **GettingStartedHost** folder, and then select **Properties** from the shortcut menu.
    a. In the **GettingStartedHost** window, for **Startup object**, select **Service.Program** (or the entry point for your particular application) from the list.
    b. From the main menu, select **File** > **Save All**.

### Run the service application

**HTTP could not register URL 'http:\//+:8000/GettingStarted/CalculatorService'. Your process does not have access rights to this namespace.**

 For proper access, start the process hosting the Windows Communication Foundation (WCF) service with administrative privileges:

- For Visual Studio: Select the Visual Studio program in the **Start** menu, and then select **More** > **Run as administrator** from the shortcut menu.
- For a console window: Select **Command Prompt** in the **Start** menu, and then select **More** > **Run As administrator** from the shortcut menu.
- For Windows Explorer: Select the executable, and then select **Run as administrator** from the shortcut menu.

### Compile the client application

**'CalculatorClient', does not contain a definition for '\<method name>' and no extension method '\<method name>' accepting a first argument of type 'CalculatorClient' could be found (are you missing a using directive or an assembly reference?)**  

Only those methods that you mark with the `ServiceOperationAttribute` attribute are publicly exposed. If you omit the `ServiceOperationAttribute` attribute from a method in the `ICalculator` interface, you receive this error message during compilation.  

**The type or namespace name 'CalculatorClient' could not be found (are you missing a using directive or an assembly reference?)**

 You receive this error if you don't add the *generatedProxy.cs* (or *generatedProxy.vb*) file to your client project when you generated them with the *Svcutil.exe* tool.  

### Run the client application

**Unhandled Exception: System.ServiceModel.EndpointNotFoundException: Could not connect to 'http:\//localhost:8000/GettingStarted/CalculatorService'. TCP error code 10061: No connection could be made because the target machine actively refused it.**

This error occurs if you run the client application without first starting the service. First, run the host application to start the service, and then run the client application.

### Use the Svcutil.exe tool

**'Svcutil' is not recognized as an internal or external command, operable program, or batch file.**

 *Svcutil.exe* must be in the system path. The easiest solution is to use the Visual Studio command prompt. From the **Start** menu, select the **Visual Studio \<version>** directory, then select **Developer Command Prompt for VS \<version>**. This command prompt sets the system path to the correct locations for all tools shipped as part of Visual Studio.  
  
### Run the service and client applications

**System.ServiceModel.Security.SecurityNegotiationException: SOAP security negotiation with 'http:\//localhost:8000/GettingStarted/CalculatorService' for target 'http:\//localhost:8000/GettingStarted/CalculatorService' failed**  

This error occurs on a domain-joined computer that doesn't have network connectivity. Connect your computer to the network or turn off security for both the service and the client.

To turn off security:

- For the service, replace the code that creates the `WSHttpBinding` with the following code:  
  
    ```csharp
    // Step 3: Add a service endpoint.
    selfhost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(SecurityMode.None), "CalculatorService");  
    ```

- For the client, in the configuration file, update the **\<security>** element under the **\<binding>** element as follows:  
  
    ```xml
    <binding name="WSHttpBinding_ICalculator">
      <security mode="None" />
    </binding>
    ```  

## See also  

 [Get started with WCF applications](getting-started-tutorial.md)  
 [WCF troubleshooting quickstart](wcf-troubleshooting-quickstart.md)  
 [Troubleshooting setup issues](troubleshooting-setup-issues.md)