---
title: "explicit (C# 參考) | Microsoft Docs"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- explicit_CSharpKeyword
- explicit
dev_langs:
- CSharp
helpviewer_keywords:
- explicit keyword [C#]
ms.assetid: cfb8f42a-e411-4db2-af9b-796b05644846
caps.latest.revision: 21
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: a5c842eba24f0a30d357afc24aeed10c1447345e
ms.lasthandoff: 03/13/2017

---
# <a name="explicit-c-reference"></a>explicit (C# 參考)
`explicit` 關鍵字會宣告必須使用轉換叫用的使用者定義型別轉換運算子。 例如，此運算子會將稱為華氏的類別轉換成稱為攝氏的類別︰  
  
 [!code-cs[csrefKeywordsConversion#2](../../../csharp/language-reference/keywords/codesnippet/CSharp/explicit_1.cs)]  
  
 此轉換運算子可以這樣叫用︰  
  
 [!code-cs[csrefKeywordsConversion#3](../../../csharp/language-reference/keywords/codesnippet/CSharp/explicit_2.cs)]  
  
 轉換運算子會將來源類型轉換成目標型別。 來源類型提供轉換運算子。 不同於隱含轉換，明確轉換運算子必須透過轉換來叫用。 如果轉換作業會造成例外狀況或遺失資訊，您應將其標記為 `explicit`。 如此可避免編譯器以無訊息方式叫用轉換作業，發生難以預料的後果。  
  
 省略轉換會導致編譯時期錯誤 CS0266。  
  
 如需詳細資訊，請參閱[使用轉換運算子](../../../csharp/programming-guide/statements-expressions-operators/using-conversion-operators.md)。  
  
## <a name="example"></a>範例  
 下例提供 `Fahrenheit` 和 `Celsius` 類別，它們互相提供明確轉換運算子。  
  
 [!code-cs[csrefKeywordsConversion#1](../../../csharp/language-reference/keywords/codesnippet/CSharp/explicit_3.cs)]  
  
## <a name="example"></a>範例  
 下例定義 `Digit` 結構，表示單一十進位數字。 已定義將 `byte` 轉換成 `Digit` 的運算子，但是因為並非所有位元組都可轉換成 `Digit`，所以是明確轉換。  
  
 [!code-cs[csrefKeywordsConversion#4](../../../csharp/language-reference/keywords/codesnippet/CSharp/explicit_4.cs)]  
  
## <a name="c-language-specification"></a>C# 語言規格  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## <a name="see-also"></a>另請參閱  
 [C# 參考](../../../csharp/language-reference/index.md)   
 [C# 程式設計手冊](../../../csharp/programming-guide/index.md)   
 [C# 關鍵字](../../../csharp/language-reference/keywords/index.md)   
 [implicit](../../../csharp/language-reference/keywords/implicit.md)   
 [運算子 (C# 參考)](../../../csharp/language-reference/keywords/operator.md)   
 [如何：在結構之間實作使用者定義的轉換](../../../csharp/programming-guide/statements-expressions-operators/how-to-implement-user-defined-conversions-between-structs.md)   
 [C# 中鏈結的使用者定義明確轉換](http://go.microsoft.com/fwlink/?LinkId=112384)
