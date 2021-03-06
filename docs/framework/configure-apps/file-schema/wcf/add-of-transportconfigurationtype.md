---
title: "&lt;transportConfigurationType&gt; 的 &lt;add&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
ms.assetid: 03d79db9-571d-4534-acef-d05e5467b257
caps.latest.revision: 12
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 12
---
# &lt;transportConfigurationType&gt; 的 &lt;add&gt;
此項目是索引鍵\/值組，可用來識別特定傳輸的型別。  
  
## 語法  
  
```  
  
<serviceHostingEnvironment>   
   <transportConfigurationTypes>  
      <add name="String"  
               transportConfigurationType="String"/>   
   </transportConfigurationTypes>  
</serviceHostingEnvironment>  
```  
  
## 屬性和項目  
 下列章節說明屬性、子項目和父項目。  
  
### 屬性  
  
|屬性|描述|  
|--------|--------|  
|name|必要的 String 屬性。<br /><br /> 包含唯一識別傳輸型別的使用者定義索引鍵。|  
|transportConfigurationType|字串，包含可實作特定傳輸的型別。|  
  
### 子項目  
 無  
  
### 父項目  
  
|項目|描述|  
|--------|--------|  
|[\<transportConfigurationTypes\>](../../../../../docs/framework/configure-apps/file-schema/wcf/transportconfigurationtypes.md)|實作特定傳輸之型別的集合。|  
  
## 範例  
  
```  
<serviceHostingEnvironment>   
   <transportConfigurationTypes>  
      <add name="net.udp"  
      transportConfigurationType="Microsoft.ServiceModel.Samples.Hosting.HostedUdpTransportConfiguration, UdpActivation, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6fa904d2da1848d6" />   
   </transportConfigurationTypes>  
</serviceHostingEnvironment>  
```  
  
## 請參閱  
 <xref:System.ServiceModel.Configuration.TransportConfigurationTypeElement>   
 <xref:System.ServiceModel.Configuration.ServiceHostingEnvironmentSection>   
 <xref:System.ServiceModel.ServiceHostingEnvironment>   
 [裝載](../../../../../docs/framework/wcf/feature-details/hosting.md)