---
title: "屬性"
description: "屬性"
keywords: .NET, .NET Core
author: BillWagner
ms.author: wiwagn
ms.date: 06/20/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 6950d25a-bba1-4744-b7c7-a3cc90438c55
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 871beb36f9801a0456eec1501fdbf07375c9b418
ms.lasthandoff: 03/13/2017

---

# <a name="properties"></a>屬性

屬性是 C# 中的頭等成員。 該語言所定義的語法，可讓開發人員撰寫正確表示其設計意圖的程式碼。

屬性在存取時的行為與欄位一樣。
不過不同於欄位，屬性是透過存取子來實作，這些存取子會定義存取或指派屬性時所執行的陳述式。

## <a name="property-syntax"></a>屬性語法
屬性的語法是欄位的自然延伸。 欄位可定義儲存位置：

```csharp
public class Person
{
    public string FirstName;
    // remaining implementation removed from listing
}
```

屬性定義包含 `get` 和 `set` 存取子的宣告，以擷取和指派該屬性的值：

```csharp
public class Person
{
    public string FirstName
    {
        get;
        set;
    }
    // remaining implementation removed from listing
}
```

以上所示的語法為*「Auto 屬性」*語法。 編譯器會為欄位產生儲存位置，以備份此屬性。 編譯器也會實作 `get` 和 `set` 存取子的主體。
您也可以自行定義儲存體，如下所示：

```csharp
public class Person
{
    public string FirstName
    {
        get { return firstName; }
        set { firstName = value; }
    }
    private string firstName;
    // remaining implementation removed from listing
}
```
 
以上所示的屬性定義為讀寫屬性。 請注意 set 存取子中的關鍵字 `value`。 `set` 存取子一定會有一個名為 `value` 的參數。 `get` 存取子必須傳回可轉換成屬性類型 (在此範例中為 `string`) 的值。
 
這是此語法的基本概念。 還有許多不同的變化，可支援各種不同的設計慣例。 讓我們探索並了解各自的語法選項。 

## <a name="scenarios"></a>案例

上述範例顯示屬性定義的一個最簡單案例，那就是不具驗證的讀/寫屬性。 藉由在 `get` 和 `set` 存取子中撰寫您所需的程式碼，您就可以建立許多不同的案例。  

### <a name="validation"></a>驗證

您可以在 `set` 存取子中撰寫程式碼，以確保由屬性所表示的值一定有效。 例如，假設 `Person` 類別的一項規則是名稱不可為空白或空白字元。 您可以撰寫如下：

```csharp
public class Person
{
    public string FirstName
    {
        get { return firstName; }
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("First name must not be blank");
            firstName = value;
        }
    }
    private string firstName;
    // remaining implementation removed from listing
}
```

上述範例會強制執行第一個名稱不得為空白或空白字元的規則。 如果開發人員撰寫
```csharp
hero.FirstName = "";
```
該指派會擲回 `ArgumentException`。 由於屬性的 set 存取子必須有無效的傳回型別，因此您擲回一個例外狀況來回報 set 存取子中發生錯誤。

這是簡單的驗證案例。 您可以將同一個語法擴充為案例所需的任何語法。 您可以檢查不同屬性之間的關聯性，或針對任何外部條件進行驗證。 任何有效的 C# 陳述式在屬性存取子中都是有效的。

### <a name="read-only"></a>唯讀

到目前為止，您所看到的所有屬性定義都是具有 public 存取子的讀取/寫入屬性。 這不是屬性唯一有效的存取範圍。
您可以建立唯讀屬性，或是為 set 和 get 存取子提供不同的存取範圍。 假設您的 `Person` 類別只能允許從該類別中的其他方法變更 `FirstName` 屬性的值。 您可以為 set 存取子提供 `private` 存取範圍，而不是 `public`：

```csharp
public class Person
{
    public string FirstName
    {
        get;
        private set;
    }
    // remaining implementation removed from listing
}
```

現在，`FirstName` 屬性可從任何程式碼存取，但只能從 `Person` 類別中的其他程式碼指派。
您可以將任何嚴格的存取修飾詞新增至 set 或 get 存取子。 您放在個別存取子上的任何存取修飾詞，必須比屬性定義上的存取修飾詞具有更多限制。 由於 `FirstName` 屬性為 `public`，但 set 存取子為 `private`，因此上述範例合法。 您無法使用 `public` 存取子宣告 `private` 屬性。 屬性宣告也可以宣告為 `protected`、`internal`、`protected internal`，甚至是 `private`。   

您也可以將更嚴格的修飾詞放在 `get` 存取子上。 例如，您可能會有 `public` 屬性，但將 `get` 存取子限制為 `private`。 這種情況在實務上很罕見。
 
### <a name="computed-properties"></a>計算的屬性

屬性不需要直接傳回成員欄位的值。 您可以建立傳回計算值的屬性。 讓我們展開 `Person` 物件，以傳回串連名字和姓氏計算得到的完整名稱：

```csharp
public class Person
{
    public string FirstName
    {
        get;
        set;
    }

    public string LastName
    {
        get;
        set;
    }

    public string FullName
    {
        get
        {
            return $"{FirstName} {LastName}";
        }
    }
}
```

上述範例使用*「字串插值」*語法，來建立完整名稱的格式化字串。

您也可以使用*「運算式主體成員」*，以提供更簡潔的方法來建立計算的 `FullName` 屬性：

```csharp
public class Person
{
    public string FirstName
    {
        get;
        set;
    }

    public string LastName
    {
        get;
        set;
    }

    public string FullName =>  $"{FirstName} {LastName}";
}
```
 
*「運算式主體成員」*使用*「Lambda 運算式」*語法來定義包含單一運算式的方法。 在這裡，該運算式會傳回 person 物件的完整名稱。

### <a name="lazy-evaluated-properties"></a>延遲評估的屬性

您可以混合計算的屬性與儲存體的概念，然後建立*「延遲評估的屬性」*。  例如，您可以更新 `FullName` 屬性，只在第一次存取時設定字串格式：

```csharp
public class Person
{
    public string FirstName
    {
        get;
        set;
    }

    public string LastName
    {
        get;
        set;
    }

    private string fullName;
    public string FullName
    {
        get
        {
            if (fullName == null)
                fullName = $"{FirstName} {LastName}";
            return fullName;
        }
    }
}
```

不過，上述程式碼含有 Bug。 如果程式碼更新 `FirstName` 或 `LastName` 屬性的值，先前評估的 `fullName` 欄位會無效。 您必須更新 `FirstName` 和 `LastName` 屬性的 `set` 存取子，以便重新計算 `fullName`欄位：

```csharp
public class Person
{
    private string firstName;
    public string FirstName
    {
        get { return firstName; }
        set
        {
            firstName = value;
            fullName = null;
        }
    }

    private string lastName;
    public string LastName
    {
        get { return lastName; }
        set
        {
            lastName = value;
            fullName = null;
        }
    }

    private string fullName;
    public string FullName
    {
        get
        {
            if (fullName == null)
                fullName = $"{FirstName} {LastName}";
            return fullName;
        }
    }
}
```

這個最終版本只會在必要時評估 `FullName` 屬性。
如果先前計算的版本有效，則會使用該版本。 如果另一個狀態變更使得先前計算的版本失效，則會重新計算。 使用此類別的開發人員不需要知道實作的細節。 這些內部變更不會影響 Person 物件的使用。 這是使用屬性來公開物件之資料成員的主要原因。 
 
### <a name="inotifypropertychanged"></a>INotifyPropertyChanged

您必須在屬性存取子中撰寫程式碼的最後一個案例是為了支援 `INotifyPropertyChanged` 介面，該介面可用來通知資料繫結用戶端已有值變更。 當屬性的值變更時，物件會引發 `PropertyChanged` 事件以指出此變更。 資料繫結程式庫會接著根據該項變更來更新顯示項目。 下列程式碼示範如何針對此 person 類別的 `FirstName` 屬性實作 `INotifyPropertyChanged`。

```csharp
public class Person : INotifyPropertyChanged
{
    public string FirstName
    {
        get { return firstName; }
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("First name must not be blank");
            if (value != firstName)
            {
                PropertyChanged?.Invoke(this, 
                    new PropertyChangedEventArgs(nameof(FirstName)));
            }
            firstName = value;
        }
    }
    private string firstName;

    public event PropertyChangedEventHandler PropertyChanged;
    // remaining implementation removed from listing
}
```

`?.` 運算子稱為*「null 條件運算子」*。 它會檢查是否有 null 參考，再評估運算子的右邊。 最後結果是，如果沒有 `PropertyChanged` 事件的訂閱者，則不會執行引發事件的程式碼。 在此情況下，它會擲回 `NullReferenceException` 而不進行這項檢查。 如需詳細資訊，請參閱 [`events`](delegates-events.md) 上的頁面。 此範例也會使用新的 `nameof` 運算子，從屬性名稱符號轉換成其文字表示。
使用 `nameof` 可減少鍵入錯誤屬性名稱時所發生的錯誤。

同樣地，這是您可以在存取子中撰寫程式碼來支援所需案例的範例。

## <a name="summing-up"></a>總結 

屬性是類別或物件中的一種智慧型欄位。 從物件外部來看，屬性就像是物件中的欄位。 不過，您可以使用完整的 C# 功能選擇區來實作屬性。
您可以提供驗證、不同的存取範圍、延遲評估，或是您的案例所需的任何需求。

