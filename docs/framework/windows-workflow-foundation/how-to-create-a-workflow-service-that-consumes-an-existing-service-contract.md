---
title: "HOW TO：建立會取用現有服務合約的工作流程服務 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 11d11b59-acc4-48bf-8e4b-e97b516aa0a9
caps.latest.revision: 5
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 5
---
# HOW TO：建立會取用現有服務合約的工作流程服務
[!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 採用合約優先工作流程開發形式，可在 Web 服務和工作流程中提供更好的整合。合約優先工作流程開發工具可讓您在 Code First 中設計合約。此工具會自動在合約中的作業工具箱內產生活動範本。  
  
> [!NOTE]
>  本主題提供建立合約優先工作流程服務的逐步指示。[!INCLUDE[crabout](../../../includes/crabout-md.md)] 如需合約優先工作流程服務開發的詳細資訊，請參閱 [合約優先工作流程服務開發](../../../docs/framework/windows-workflow-foundation//contract-first-workflow-service-development.md)。  
  
### 建立工作流程專案  
  
1.  在 [!INCLUDE[vs_current_short](../../../includes/vs-current-short-md.md)] 中，依序選取 \[**檔案**\]、\[**新增專案**\]。在 \[**範本**\] 樹狀結構中選取 \[**C\#**\] 節點下的 \[**WCF**\] 節點，然後選取 \[**WCF 工作流程服務應用程式**\] 範本。  
  
2.  將新的專案命名為 `ContractFirst`，再按一下 \[**確定**\]。  
  
### 建立服務合約  
  
1.  在 \[**方案總管**\] 中，以滑鼠右鍵按一下專案，然後依序選取 \[**加入**\]、\[**新增項目**\]。選取位在左側的 \[**程式碼**\] 節點和位在右側的 \[**類別**\] 範本。將新類別命名為 `IBookService`，然後按一下 \[**確定**\]。  
  
2.  在出現的程式碼視窗最上方，將 Using 陳述式加入到 `System.Servicemodel`。  
  
    ```  
    using System.ServiceModel;  
    ```  
  
3.  將範例類別定義變更為下列介面定義。  
  
    ```  
    [ServiceContract]  
        public interface IBookService  
        {  
            [OperationContract]  
            void Buy(string bookName);  
  
            [OperationContract(IsOneWay=true)]  
            void Checkout();  
        }  
    ```  
  
4.  按一下 \[**Ctrl\+Shift\+B**\] 建置此專案。  
  
### 匯入服務合約  
  
1.  在 \[**方案總管**\] 中，以滑鼠右鍵按一下專案，然後選取 \[**匯入服務合約**\]。開啟 \[**\<目前專案\>**\] 下的所有子節點，再選取 \[**IBookService**\]。按一下 \[**確定**\]。  
  
2.  隨即會開啟一個對話方塊，警告您作業已成功完成，且所產生的活動將會在您建置專案後出現在工具箱中。按一下 \[**確定**\]。  
  
3.  按下 \[**Ctrl\+Shift\+B**\] 建置專案，匯入活動將出現在工具箱中。  
  
4.  在 \[**方案總管**\] 中，開啟 .Service1.xamlx。工作流程服務會出現在設計工具中。  
  
5.  選取 \[**序列**\] 活動。在 \[屬性\] 視窗中，按一下 \[**ImplementedContract**\] 屬性中的 **...** 按鈕。在出現的 \[**型別集合編輯器**\] 視窗中，按一下 \[**型別**\] 下拉式清單並選取 \[**瀏覽型別**\] 項目。在 \[**瀏覽並選取 .Net 型別**\] 對話方塊中，開啟 \[**\<目前專案\>**\] 下的所有子節點並選取 \[**IBookService**\]。按一下 \[**確定**\]。在 \[**型別集合編輯器**\] 對話方塊中，按一下 \[**確定**\]。  
  
6.  選取並刪除 \[**ReceiveRequest**\] 和 \[**SendResponse**\] 活動。  
  
7.  從工具箱拖曳 \[**Buy\_ReceiveAndSendReply**\] 和 \[**Checkout\_Receive**\] 活動到 \[**循序服務**\] 活動。