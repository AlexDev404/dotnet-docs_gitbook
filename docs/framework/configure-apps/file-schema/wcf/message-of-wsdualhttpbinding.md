---
description: "Learn more about: <message> of <wsDualHttpBinding>"
title: "<message> of <wsDualHttpBinding>"
ms.date: "03/30/2017"
ms.assetid: 75101744-eed8-4d61-91f4-5fc4473a21f2
---
# \<message> of \<wsDualHttpBinding>

Defines message-level security for the [\<wsDualHttpBinding>](wsdualhttpbinding.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<wsDualHttpBinding>**](wsdualhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-wsdualhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<message>**  
  
## Syntax  
  
```xml  
<message clientCredentialType="None/Windows/UserName/Certificate/CardSpace"
         negotiateServiceCredential="Boolean"
         algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15">
</message>
```  
  
## Type  

 <xref:System.ServiceModel.MessageSecurityOverHttp>  
  
## Attributes and Elements  

 The following sections describe attributes, child elements, and parent elements  
  
### Attributes  
  
|Attribute|Description|  
|---------------|-----------------|  
|algorithmSuite|Optional. Sets the message encryption and key-wrap algorithms. The algorithms and the key sizes are determined by the <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> class. These algorithms map to those specified in the Security Policy Language (WS-SecurityPolicy) specification.<br /><br /> See below for possible values. The default value is `Basic256`.|  
|clientCredentialType|Optional. Specifies the type of credential to be used when performing client authentication using the security mode is `Message`. See below for possible values. The default is `Windows`.<br /><br /> This attribute is of type <xref:System.ServiceModel.MessageCredentialType>.|  
|negotiateServiceCredential|Optional. A Boolean value that specifies whether the service credential is provisioned at the client out of band or is obtained from the service to the client through a process of negotiation. Such a negotiation is a precursor to the usual message exchange.<br /><br /> If the `clientCredentialType` attribute equals to None, Username, or Certificate, setting this attribute to `false` implies that the service certificate is available at the client out of band and that the client needs to specify the service certificate (using the [\<serviceCertificate>](servicecertificate-of-servicecredentials.md)) in the [\<serviceCredentials>](servicecredentials.md) service behavior. This mode is interoperable with SOAP stacks which implement WS-Trust and WS-SecureConversation.<br /><br /> If the `ClientCredentialType` attribute is set to `Windows`, setting this attribute to `false` specifies Kerberos based authentication. This means that the client and service must be part of the same Kerberos domain. This mode is interoperable with SOAP stacks which implement the Kerberos token profile (as defined at OASIS WSS TC) as well as WS-Trust and WS-SecureConversation. When this attribute is `true`, it causes a .NET SOAP negotiation that tunnels SPNego exchange over SOAP messages.<br /><br /> The default is `true`.|  
  
## algorithmSuite Attribute  
  
|Value|Description|  
|-----------|-----------------|  
|Basic128|Use Aes128 encryption, Sha1 for message digest, and Rsa-oaep-mgf1p for key wrap.|  
|Basic192|Use Aes192 encryption, Sha1 for message digest, Rsa-oaep-mgf1p for key wrap.|  
|Basic256|Use Aes256 encryption, Sha1 for message digest, Rsa-oaep-mgf1p for key wrap.|  
|Basic256Rsa15|Use Aes256 for message encryption, Sha1 for message digest and Rsa15 for key wrap.|  
|Basic192Rsa15|Use Aes192 for message encryption, Sha1 for message digest and Rsa15 for key wrap.|  
|TripleDes|Use TripleDes encryption, Sha1 for message digest, Rsa-oaep-mgf1p for key wrap.|  
|Basic128Rsa15|Use Aes128 for message encryption, Sha1 for message digest and Rsa15 for key wrap.|  
|TripleDesRsa15|Use TripleDes encryption, Sha1 for message digest and Rsa15 for key wrap.|  
|Basic128Sha256|Use Aes256 for message encryption, Sha256 for message digest and Rsa-oaep-mgf1p for key wrap.|  
|Basic192Sha256|Use Aes192 for message encryption, Sha256 for message digest and Rsa-oaep-mgf1p for key wrap.|  
|Basic256Sha256|Use Aes256 for message encryption, Sha256 for message digest and Rsa-oaep-mgf1p for key wrap.|  
|TripleDesSha256|Use TripleDes for message encryption, Sha256 for message digest and Rsa-oaep-mgf1p for key wrap.|  
|Basic128Sha256Rsa15|Use Aes128 for message encryption, Sha256 for message digest and Rsa15 for key wrap.|  
|Basic192Sha256Rsa15|Use Aes192 for message encryption, Sha256 for message digest and Rsa15 for key wrap.|  
|Basic256Sha256Rsa15|Use Aes256 for message encryption, Sha256 for message digest and Rsa15 for key wrap.|  
|TripleDesSha256Rsa15|Use TripleDes for message encryption, Sha256 for message digest and Rsa15 for key wrap.|  
  
## clientCredentialType Attribute  
  
|Value|Description|  
|-----------|-----------------|  
|None|This allows the service to interact with anonymous clients. On the service side, this indicates that the service does not require any client credential. On the client, this indicates that the client does not provide any client credential.|  
|Windows|Allows the SOAP exchanges to be under the authenticated context of a Windows credential. If the `negotiateServiceCredential` attribute is set to `true`, this either performs an SSPI Negotiation or Kerberos (an interoperable standard).|  
|UserName|Allows the service to require that the client be authenticated using a UserName credential. WCF does not support sending a password digest or deriving keys using password and using such keys for message security. As such, WCF enforces that the transport is secured when using UserName credentials. This credential mode results in either an interoperable exchange or a non-interoperable negotiation based on the `negotiateServiceCredential` attribute.|  
|Certificate|Allows the service to require that the client be authenticated using a certificate. If message security mode is used and the `negotiateServiceCredential` attribute is set to `false`, the client needs to be provisioned with the service certificate.|  
|IssuedToken|Specifies a custom token, usually issued by a Security Token Service.|  
  
### Child Elements  

 None.  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<security>](security-of-wsdualhttpbinding.md)|Defines the security capabilities of the [\<wsDualHttpBinding>](wsdualhttpbinding.md).|  
  
## See also

- <xref:System.ServiceModel.Configuration.WSDualHttpSecurityElement.Message%2A>
- <xref:System.ServiceModel.WSDualHttpSecurity.Message%2A>
- <xref:System.ServiceModel.Configuration.MessageSecurityOverTcpElement>
- <xref:System.ServiceModel.MessageSecurityOverHttp>
- [Securing Services and Clients](../../../wcf/feature-details/securing-services-and-clients.md)
- [Bindings](../../../wcf/bindings.md)
- [Configuring System-Provided Bindings](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Using Bindings to Configure Services and Clients](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
