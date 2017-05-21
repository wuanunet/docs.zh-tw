---
title: "索引子"
description: "索引子"
keywords: .NET, .NET Core
author: BillWagner
ms.author: wiwagn
ms.date: 06/20/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 0e9496da-e766-45a9-b92b-91820d4a350e
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 3dc9347aa3c4090b71d473d13b5c7ad68f1fbc76
ms.lasthandoff: 03/13/2017

---

# <a name="indexers"></a>索引子

*「索引子」*類似於屬性。 從許多方面來看，索引子是建立在與[屬性](properties.md)相同的語言功能之上。 索引子可啟用*「索引」*屬性，也就是使用一或多個引數參考的屬性。 這些引數可對某些數值集合進行索引。

## <a name="indexer-syntax"></a>索引子語法

您可以透過變數名稱和方括號來存取索引子。 您會將索引子引數放在方括號內：

```csharp
var item = someObject["key"];
someObject["AnotherKey"] = item;
```

若要宣告索引子，請使用 `this` 關鍵字作為屬性名稱，然後宣告方括號內的引數。 此宣告會比對上一個段落中所示的使用方式：

```csharp
public int this[string key]
{
    get { return storage.Find(key); }
    set { storage.SetAt(key, value); }
}
```

在第一個範例中，您可以看到屬性語法和索引子語法之間的關聯性。 這點相似性會出現在索引子的大部分語法規則中。 索引子可以有任何有效的存取修飾詞 (public、protected internal、protected、internal 或 private)。 它們可能是密封、虛擬或抽象的。 如同屬性，您可以為索引子中 get 和 set 存取子，指定不同的存取修飾詞。
您也可以指定唯讀索引子 (藉由省略 set 存取子) 或唯寫索引子 (藉由省略 get 存取子)。

您可以將使用屬性時所學到的幾乎所有知識應用到索引子。 這項規則的唯一例外是*「自動實作屬性」*。 編譯器不一定可為索引子產生正確的儲存體。

索引子和屬性的不同之處，在於具有可參考一組項目中某個項目的引數。 您可以在一個類型上定義多個索引子，但前提是每個索引子的引數清單必須是唯一的。 讓我們探索幾個不同的案例，在這些案例中，您可以在類別定義中使用一或多個索引子。 

## <a name="scenarios"></a>案例

當您的類型 API 建立一些集合的模型，以供您在其中定義該集合的引數時，您會在此類型中定義*「索引子」*。 您的索引子不一定會直接對應至屬於 .NET Core Framework 的集合類型。 您的類型除了建立集合的模型之外，可能還有其他職責。
索引子可讓您提供符合類型抽象概念的 API，而不會公開有關該抽象概念的值如何儲存或計算的內部詳細資料。

讓我們逐步解說使用*「索引子」*的一些常見案例。
您可以在 [GitHub 儲存機制](https://github.com/dotnet/core-docs)的核心文件中，存取所有範例的程式碼。 或者，您可以直接存取[範例資料夾](https://github.com/dotnet/docs/tree/master/samples/csharp/indexers)。

### <a name="arrays-and-vectors"></a>陣列和向量

建立索引子的其中一個最常見案例，就是當您的類型建立陣列或向量的模型時。 您可以建立索引子來建立已排序資料清單的模型。 

建立您自己的索引子的優點，就是您可以定義該集合的儲存體以符合您的需求。 假設您的類型所建立的歷程資料模型太大，而無法一次全部載入記憶體。 您需要根據使用情況載入和卸載集合區段。 下列範例會建立此行為的模型。 它會報告有多少資料點。 它會建立頁面，以視需要保留資料區段。 它會從記憶體中刪除頁面，以挪出空間給較新要求所需的頁面使用。

```csharp
public class DataSamples
{
    private class Page
    {
        private readonly List<Measurements> pageData = new List<Measurements>();
        private readonly int startingIndex;
        private readonly int length;
        private bool dirty;
        private DateTime lastAccess;

        public Page(int startingIndex, int length)
        {
            this.startingIndex = startingIndex;
            this.length = length;
            lastAccess = DateTime.Now;

            // This stays as random stuff:
            var generator = new Random();
            for(int i=0; i < length; i++)
            {
                var m = new Measurements
                {
                    HiTemp = generator.Next(50, 95),
                    LoTemp = generator.Next(12, 49),
                    AirPressure = 28.0 + generator.NextDouble() * 4
                };
                pageData.Add(m);
            }
        }
        public bool HasItem(int index) =>
            ((index >= startingIndex) &&
            (index < startingIndex + length));

        public Measurements this[int index]
        {
            get
            {
                lastAccess = DateTime.Now;
                return pageData[index - startingIndex];
            }
            set
            {
                pageData[index - startingIndex] = value;
                dirty = true;
                lastAccess = DateTime.Now;
            }
        }

        public bool Dirty => dirty;
        public DateTime LastAccess => lastAccess;
    }

    private readonly int totalSize;
    private readonly List<Page> pagesInMemory = new List<Page>();

    public DataSamples(int totalSize)
    {
        this.totalSize = totalSize;
    }

    public Measurements this[int index]
    {
        get
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");

            var page = updateCachedPagesForAccess(index);
            return page[index];
        }
        set
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");
            var page = updateCachedPagesForAccess(index);

            page[index] = value;
        }
    }

    private Page updateCachedPagesForAccess(int index)
    {
        foreach (var p in pagesInMemory)
        {
            if (p.HasItem(index))
            {
                return p;
            }
        }
        var startingIndex = (index / 1000) * 1000;
        var newPage = new Page(startingIndex, 1000);
        addPageToCache(newPage);
        return newPage;
    }

    private void addPageToCache(Page p)
    {
        if (pagesInMemory.Count > 4)
        {
            // remove oldest non-dirty page:
            var oldest = pagesInMemory
                .Where(page => !page.Dirty)
                .OrderBy(page => page.LastAccess)
                .FirstOrDefault();
            // Note that this may keep more than 5 pages in memory
            // if too much is dirty
            if (oldest != null)
                pagesInMemory.Remove(oldest);
        }
        pagesInMemory.Add(p);
    }
}
```

您可以遵循此設計慣例，來建立各式各樣的集合模型，在此情況下，您有很好的理由不將整組資料載入記憶體內部集合中。 請注意，`Page` 類別是不屬於公用介面的私用巢狀類別。 此類別的所有使用者都不會看到這些詳細資料。

### <a name="dictionaries"></a>字典

另一個常見案例是當您需要建立字典或對應的模型時。 當您的類型根據索引鍵 (通常是文字索引鍵) 儲存值時，就會發生此情況。 此範例會建立將命令列引數對應至管理這些選項之 [Lamdba 運算式](delegates-overview.md)的字典。 下列範例顯示兩個類別︰將命令列選項對應至 `Action` 委派的 `ArgsActions` 類別，以及遇到該選項時會使用 `ArgsActions` 執行每個 `Action` 的 `ArgsProcessor`。

```csharp
public class ArgsProcessor
{
    private readonly ArgsActions actions;

    public ArgsProcessor(ArgsActions actions)
    {
        this.actions = actions;
    }

    public void Process(string[] args)
    {
        foreach(var arg in args)
        {
            actions[arg]?.Invoke();
        }
    }

}
public class ArgsActions
{
    readonly private Dictionary<string, Action> argsActions = new Dictionary<string, Action>();

    public Action this[string s]
    {
        get
        {
            Action action;
            Action defaultAction = () => {} ;
            return argsActions.TryGetValue(s, out action) ? action : defaultAction;
        }
    }

    public void SetOption(string s, Action a)
    {
        argsActions[s] = a;
    }
}
```

在此範例中，`ArgsAction` 集合會緊密對應至基礎集合。
`get` 會判斷是否已設定指定的選項。 如果已設定，則會傳回與該選項相關聯的 `Action`。 如果未設定，則會傳回不執行任何動作的 `Action`。 公用存取子不包含 `set` 存取子。 相反地，此設計使用公用方法來設定選項。

### <a name="multi-dimensional-maps"></a>多維對應

您可以建立使用多個引數的索引子。 此外，這些引數並不限於相同的類型。 讓我們來看以下兩個範例。   

第一個範例顯示產生 Mandelbrot 集合值的類別。 如需該集合背後數學的詳細資訊，請參閱[這篇文章](https://en.wikipedia.org/wiki/Mandelbrot_set)。 索引子使用兩個雙精度浮點數在 X, Y 平面中定義一個點。
get 存取子會計算反覆運算次數，直到某個點經判斷不在集合內為止。 如果到達最高次數，該點是在集合內，而且會傳回類別的 maxIterations 值 (為了 Mandelbrot 集合所普及化的電腦產生影像，會定義決定點在集合之外所需之反覆運算次數的色彩)。

```csharp
public class Mandelbrot
{
    readonly private int maxIterations;

    public Mandelbrot(int maxIterations)
    {
        this.maxIterations = maxIterations;
    }

    public int this [double x, double y]
    {
        get
        {
            var iterations = 0;
            var x0 = x;
            var y0 = y;

            while ((x*x + y * y < 4) &&
                (iterations < maxIterations))
            {
                var newX = x * x - y * y + x0;
                y = 2 * x * y + y0;
                x = newX;
                iterations++;
            }
            return iterations;
        }
    }
}
```

Mandelbrot 集合會在每個 (x,y) 座標定義實數值的值。
這會定義可能包含無限數目之值的字典。 因此，該集合背後沒有任何儲存體。 相反地，當程式碼呼叫 `get` 存取子時，此類別會計算每個點的值； 但不會使用任何基礎儲存體。

讓我們來看上次使用的索引子，該索引子接受多個不同類型的引數。 假設有個管理歷程溫度資料的程式。 此索引子使用城市和日期來設定或取得該地點的高低溫度：

```csharp
using DateMeasurements = 
    System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements = 
    System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;

public class HistoricalWeatherData
{
    readonly CityDataMeasurements storage = new CityDataMeasurements();

    public Measurements this[string city, DateTime date]
    {
        get
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
                throw new ArgumentOutOfRangeException(nameof(city), "City not found");

            // strip out any time portion:
            var index = date.Date;
            var measure = default(Measurements);
            if (cityData.TryGetValue(index, out measure))
                return measure;
            throw new ArgumentOutOfRangeException(nameof(date), "Date not found");
        }
        set
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
            {
                cityData = new DateMeasurements();
                storage.Add(city, cityData);
            }

            // Strip out any time portion:
            var index = date.Date;
            cityData[index] = value;
        }
    }
}
```

此範例會建立對應兩個不同引數之天氣資料的索引子︰城市 (以 `string` 表示) 和日期 (以 `DateTime` 表示)。 內部儲存體使用兩個 `Dictionary` 類別來代表二維字典。 此公用 API 不再代表基礎儲存體。 相反地，索引子的語言功能可讓您建立公用介面來代表您的抽象概念，不過基礎儲存體必須使用不同的核心集合類型。

有些開發人員對此程式碼的其中兩部分可能不熟悉。 這兩個 `using` 陳述式：

```csharp
using DateMeasurements = System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements = System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;
```

會為建構的泛型型別建立*「別名」*。 這些陳述式可讓程式碼稍後使用更具描述性的 `DateMeasurements` 和 `CityDateMeasurements` 名稱，而不是 `Dictionary<DateTime, Measurements>` 和 `Dictionary<string, Dictionary<DateTime, Measurements> >` 的泛型建構。 此建構確實需要在 `=` 符號右側使用完整的類型名稱。

第二個方法是移除用來對集合進行索引之任何 `DateTime` 物件的時間部分。 .NET Framework 不包含只有日期的類型。
開發人員使用 `DateTime` 類型，但使用 `Date` 屬性來確保該日期的任何 `DateTime` 物件都相等。

## <a name="summing-up"></a>總結

每當您的類別中有屬性類元素，且該屬性不代表單一值，而代表數值集合 (其中每個項目是由一組引數所識別) 時，就應該建立索引子。 這些引數可唯一識別應參考集合中的哪個項目。
索引子延伸[屬性](properties.md)的概念，其中的成員會當做類別外部的資料項目來處理，但索引子類似一旁的方法。 索引子允許引數在代表一組項目的屬性中尋找單一項目。

