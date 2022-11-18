---
title: "Enabling Network Tracing"
description: Learn how to enable network tracing, which provides information about method invocations and network traffic for a managed application in the .NET Framework.
ms.date: "03/30/2017"
helpviewer_keywords: 
  - "trace destinations"
  - "sending traces to log file"
  - "trace listeners, network tracing"
  - "network tracing, enabling"
  - "CLR Debugger"
  - "default listeners"
  - "logs, trace"
  - "destination for tracing output"
ms.topic: how-to
---
# Enabling Network Tracing

Network tracing provides access to information about method invocations and network traffic generated by a managed application. You must complete the following tasks to enable network tracing in your application:  
  
- Compile your code with tracing enabled. See [How to: Compile Conditionally with Trace and Debug](../debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md) for more information about the compiler switches required to enable tracing.  
  
- Specify a destination for tracing output.  
  
- Configure the behavior of network tracing. See [How to: Configure Network Tracing](how-to-configure-network-tracing.md) for detailed information.  
  
 The most common trace destinations, also referred to as trace listeners, are the default listener and the log file.  
  
 Tracing uses the default listener if you do not specify a trace listener. You can view messages sent to the default listener by executing your code in a managed code-enabled debugger such as the CLR Debugger shipped with the .NET Framework SDK, or DBwin32.exe shipped with the Windows SDK. Using the CLR Debugger, the trace messages appear in the **Output** window.  
  
 If you prefer to use a file to receive traces, you can specify a log file by using configuration settings, as shown in the following example. (For a general discussion of configuration files, see [Configuration Files](../configure-apps/index.md).)  
  
 To send traces to a log file, add the following node to the `<system.diagnostics>` node of the appropriate configuration file (application or machine). You can change the name of the file (trace.log) to suit your needs.  
  
```xml  
<system.diagnostics>  
  <trace autoflush="true" indentsize="4">  
    <listeners>  
      <add name="file" type="System.Diagnostics.TextWriterTraceListener" initializeData="trace.log"/>  
    </listeners>
  </trace>  
</system.diagnostics>  
```  
  
## See also

- [Interpreting Network Tracing](interpreting-network-tracing.md)
- [Network Tracing in the .NET Framework](network-tracing.md)
- [Tracing and Instrumenting Applications](../debug-trace-profile/tracing-and-instrumenting-applications.md)