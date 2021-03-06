---
title: "&lt;claimTypeRequirements&gt; 項目的 &lt;add&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 3234cd45-1478-468e-8b19-5c50815c4786
caps.latest.revision: 5
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 5
---
# &lt;claimTypeRequirements&gt; 項目的 &lt;add&gt;
指定必須在聯合認證中出現的必要及選擇性宣告型別。  例如，服務說明傳入認證必須處理特定的一組宣告型別。  
  
## 語法  
  
```  
  
<claimTypeRequirements>  
      <add claimType="URI"  
        isOptional="Boolean" />  
</claimTypeRequirements>  
```  
  
## 屬性和項目  
 下列章節說明屬性、子項目和父項目。  
  
### 屬性  
  
|屬性|描述|  
|--------|--------|  
|claimType|定義宣告型別的 URI。  例如，要在網站上購物，使用者必須提供具有足夠信用額度的有效信用卡。  宣告型別是信用卡的 URI。|  
|isOptional|布林值，指定此宣告是否為選擇性宣告。  若此為必要宣告，請將此屬性設為 `false`。<br /><br /> 若服務要求某些資訊，但並非必要，便可使用此屬性。  例如，若您要求使用者輸入姓氏、名字和地址，但決定電話號碼為選擇性。|  
  
### 子項目  
 無。  
  
### 父項目  
  
|項目|描述|  
|--------|--------|  
|[\<claimTypeRequirements\>](../../../../../docs/framework/configure-apps/file-schema/wcf/claimtyperequirements-for-message.md)|指定必要宣告型別的集合。  每個項目的型別為 <xref:System.ServiceModel.Configuration.ClaimTypeElement>。<br /><br /> 在聯合案例中，服務會聲明對傳入認證的需求。  例如，傳入認證必須處理特定的一組宣告型別。  這個集合中的每一個項目都會指定要顯示在聯合認證中的必要和選擇性宣告型別。|  
  
## 備註  
 在聯合案例中，服務會聲明對傳入認證的需求。  例如，傳入認證必須處理特定的一組宣告型別。  這項需求會顯示在安全性原則中。  當用戶端從聯合服務 \(例如 CardSpace\) 要求認證，它會將需求放入權杖要求 \(RequestSecurityToken\)，那麼聯合服務便可發出認證，滿足相應的需求。  
  
## 範例  
 下列組態會將兩項宣告型別需求新增至安全性繫結。  
  
```  
<bindings>  
    <wsFederationHttpBinding>  
      <binding name="myFederatedBinding">  
        <security mode="Message">  
          <message issuedTokenType="urn:oasis:names:tc:SAML:1.0:assertion">  
            <claimTypeRequirements>  
              <add claimType=  
"http://schemas.microsoft.com/ws/2005/05/identity/claims/EmailAddress"/>  
              <add claimType=  
"http://schemas.microsoft.com/ws/2005/05/identity/claims/UserName"    
optional="true" />  
            </claims>  
          </message>  
        </security>  
      </binding>  
    </wsFederationHttpBinding>  
</bindings>  
```  
  
## 請參閱  
 <xref:System.ServiceModel.FederatedMessageSecurityOverHttp.ClaimTypeRequirements%2A>   
 <xref:System.ServiceModel.Security.Tokens.ClaimTypeRequirement>   
 <xref:System.ServiceModel.Configuration.FederatedMessageSecurityOverHttpElement.ClaimTypeRequirements%2A>   
 <xref:System.ServiceModel.Configuration.ClaimTypeElementCollection>   
 <xref:System.ServiceModel.Configuration.ClaimTypeElement>