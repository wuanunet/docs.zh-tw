---
title: "將服務參考加入至可攜式子集專案 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 61ccfe0f-a34b-40ca-8f5e-725fa1b8095e
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 將服務參考加入至可攜式子集專案
可攜式子集專案可讓 .NET 組件程式設計人員維護單一來源的樹狀目錄和建置系統，同時還支援多個 .NET 平台 \(桌上型電腦、Silverlight、Windows Phone 和 XBOX\)。可攜式子集專案僅參考 .NET 可攜式程式庫，這些程式庫是可用於任何核心 .NET 平台的 .NET Framework 組件。  
  
## 加入服務參考詳細資料  
 將服務參考加入至可攜式子集專案時會強制執行下列限制：  
  
1.  <xref:System.Xml.Serialization.XmlSerializer> 只能使用常值編碼。SOAP 編碼會在匯出期間產生錯誤。  
  
2.  對於使用 <xref:System.Runtime.Serialization.DataContractSerializer> 案例的服務，會提供資料合約 Surrogate 以確保重複使用的型別只能來自可攜式子集。  
  
3.  可攜式程式庫不支援需要依賴繫結的端點 \(所有繫結，但是不包括 <xref:System.ServiceModel.BasicHttpBinding>、沒有交易流程的 <xref:System.ServiceModel.WsHttpBinding>、可靠工作階段或 MTOM 編碼和對等的自訂繫結\)，將會被忽略。  
  
4.  在匯出之前，會刪除所有作業中全部訊息描述之訊息標頭。  
  
5.  會從產生的用戶端 Proxy 程式碼中移除非可攜式屬性 <xref:System.ComponentModel.DesignerCategoryAttribute>、<xref:System.Serializable> 和 <xref:System.ServiceModel.TransactionFlow>。  
  
6.  會從 <xref:System.ServiceModel.ServiceContractAttribute>、<xref:System.ServiceModel.OperationContract> 和 <xref:System.ServiceModel.FaultContract> 中移除非可攜式屬性 ProtectionLevel、SessionMode、IsInitiating 和 IsTerminating。  
  
7.  所有服務作業都會產生為用戶端 Proxy 上的非同步作業。  
  
8.  會將所產生且使用非可攜式型別的用戶端建構函式移除。  
  
9. <xref:System.Net.CookieContainer> 執行個體會在產生的用戶端上公開。  
  
10. 會在識別程式碼產生器組件及版本的檔案頂端插入註解：`// This code was auto-generated by Microsoft.VisualStudio.Portable.AddServiceReference, version 1.0.0.0`  
  
11. 不支援 <xref:System.Runtime.Serialization.ISerializable> 介面。  
  
12. 不支援 Net.Tcp 和 PollingDuplex 繫結  
  
13. 永遠使用 <xref:System.Runtime.Serialization.DataContractSerializer> 處理錯誤。  
  
14. 在可攜式子集專案中不支援 <xref:System.ServiceModel.MessageContractAttribute.IsWrapped%2A>。  
  
## 請參閱  
 [使用 WCF 用戶端存取服務](../../../docs/framework/wcf/accessing-services-using-a-wcf-client.md)   
 [可攜式類別庫](http://msdn.microsoft.com/library/gg597391\(v=vs.110\))