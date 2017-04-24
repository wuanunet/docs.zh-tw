---
title: "建構函式設計 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "成員設計方針，建構函式"
  - "建構函式，設計方針"
  - "執行個體建構函式"
  - "型別建構函式"
  - "虛擬成員"
  - "型別建構函式"
  - "預設建構函式"
  - "靜態建構函式"
ms.assetid: b4496afe-5fa7-4bb0-85ca-70b0ef21e6fc
caps.latest.revision: 12
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 12
---
# 建構函式設計
有兩種類型的建構函式︰ 建構函式和執行個體建構函式的輸入。  
  
 型別建構函式是靜態的並使用型別之前，由 CLR 執行。 建立型別的執行個體時，就會執行執行個體建構函式。  
  
 型別建構函式不接受任何參數。 可以執行個體建構函式。 未採用任何參數的執行個體建構函式通常稱為 「 預設建構函式。  
  
 建構函式是最自然的方式，來建立型別的執行個體。 大部分的開發人員將搜尋，並嘗試使用建構函式，這些考量的替代方式來建立執行個體 （例如 factory 方法） 之前。  
  
 **✓ 考慮** 提供簡單、 理想的情況下預設、 建構函式。  
  
 簡單的建構函式非常少量的參數，而且所有參數都是基本型別或列舉型別。 這類簡單的建構函式提升可用性架構。  
  
 **✓ 考慮** 使用的靜態 factory 方法，而不建構函式，如果所需作業的語意不會對應至建構的新執行個體，直接或建構函式設計指導方針並不適合。  
  
 **✓ 執行** 以捷徑使用建構函式參數，設定主要屬性。  
  
 不應該有任何差異，在語意使用空的建構函式後面接著一些屬性集，以及使用多個引數的建構函式。  
  
 **✓ 執行** 如果建構函式參數用來直接設定屬性，請使用相同名稱的建構函式參數和屬性。  
  
 這類參數和屬性之間唯一的差別應該大小寫。  
  
 **✓ 執行** 最少的工作，在建構函式。  
  
 建構函式應該做的工作，擷取以外的建構函式參數。 任何其他處理的成本應該會延遲，直到所需。  
  
 **✓ 執行** 擲回例外狀況，從執行個體建構函式，如果適用的話。  
  
 **✓ 執行** 明確宣告公用預設建構函式，在類別中，如果需要這類建構函式。  
  
 如果您沒有明確宣告任何建構函式，型別上，許多語言 （例如 C\# 中\) 會自動加入公用預設建構函式。 （抽象類別取得受保護的建構函式）。  
  
 將參數化建構函式加入至類別，可防止編譯器新增預設建構函式。 這通常會導致意外的重大變更。  
  
 **X 避免** 明確定義在結構上的預設建構函式。  
  
 這對陣列建立速度更快，因為如果未定義預設建構函式，它並沒有在陣列中每個位置上執行。 請注意，許多編譯器，包括 C\# 中，不允許結構，因此具有無參數建構函式。  
  
 **X 避免** 呼叫其建構函式內的物件上的虛擬成員。  
  
 呼叫虛擬成員會導致最具衍生性的覆寫，會呼叫，即使最具衍生性的型別建構函式已完全尚未執行。  
  
### 型別建構函式的指導方針  
 **✓ 執行** 讓靜態建構函式成為 private。  
  
 靜態建構函式，也稱為類別的建構函式，用於初始化型別。 CLR 會建立型別的第一個執行個體或呼叫該型別的任何靜態成員之前呼叫靜態建構函式。 使用者將無法控制當呼叫靜態建構函式。 如果靜態建構函式不是私用，則可以由 CLR 以外的程式碼呼叫它。 根據建構函式中執行的作業，這可能會造成非預期的行為。 C\# 編譯器會強制為私用的靜態建構函式。  
  
 **X 不** 擲回例外狀況從靜態建構函式。  
  
 如果從型別建構函式擲回例外狀況，類型不使用目前的應用程式定義域中。  
  
 **✓ 考慮** 初始化的靜態欄位內嵌，而不是使用明確靜態建構函式，因為執行階段能夠使效能最佳化的不需要明確定義的靜態建構函式的類型。  
  
 *部分 © 2005年、 2009 Microsoft Corporation。 著作權所有，並保留一切權利。*  
  
 *皮耳森教育，從 Inc.的權限所印製 [Framework 設計方針︰ 慣例、 慣用句和可重複使用.NET 程式庫，第 2 版的模式](http://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) Krzysztof Cwalina 並 Brad Abrams，2008 年 10 月 22 日由 Addison\-wesley Professional 的 Microsoft Windows 開發系列的一部分發行。*  
  
## 請參閱  
 [成員設計方針](../../../docs/standard/design-guidelines/member.md)   
 [Framework 設計方針](../../../docs/standard/design-guidelines/index.md)