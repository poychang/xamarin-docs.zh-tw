---
title: 與 Xamarin.iOS 使用 SQLite.NET
description: SQLite.NET PCL 的 NuGet 程式庫提供一種簡單的資料存取機制 Xamarin.iOS 應用程式。 本文件提供如何使用此程式庫的概觀。
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/18/2018
ms.openlocfilehash: e78c224bae3a0e2c2dfcfded30a4bf2c4794e255
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112009"
---
# <a name="using-sqlitenet-with-xamarinios"></a>與 Xamarin.iOS 使用 SQLite.NET

Xamarin 會建議使用 SQLite.NET 程式庫是基本的 ORM，可讓您儲存和擷取物件之 iOS 裝置上的本機 SQLite 資料庫中。
ORM 代表物件關聯式對應 – 可讓您儲存和擷取資料庫中的 「 物件 」，而不需要撰寫 SQL 陳述式的 API。

<a name="Usage"/>

## <a name="usage"></a>使用量

若要包含 SQLite.NET 程式庫中的 Xamarin 應用程式，將下列 NuGet 套件加入專案：

- **套件名稱：** sqlite net pcl
- **作者：** Frank A.Krueger
- **識別碼：** sqlite net pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet 套件](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet 套件")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> 有幾個不同的 SQLite 套件的 – 務必選擇正確的密碼 （可能不在搜尋中的第一筆結果）。

使用 SQLite.NET 文件庫之後，請遵循使用它來存取資料庫的這三個步驟：

1. **加入 using 陳述式**-加入下列陳述式，C#檔案需要的資料存取的位置：

    ```csharp
    using SQLite;
    ```

1. **建立空白資料庫**-SQLiteConnection 類別建構函式傳遞的檔案路徑也可以建立的資料庫參考。 您不需要檢查如果檔案已經存在，它會自動建立是否必要，否則現有的資料庫檔案會開啟。

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

    根據本文件稍早所述的規則應該判斷 dbPath 變數。

1. **將資料儲存**-一旦您已建立 SQLiteConnection 物件，藉由呼叫其方法，例如 CreateTable 和插入，這類執行命令的資料庫：

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

1. **擷取資料**-以擷取物件 （或物件的清單） 使用下列語法：

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>基本資料存取範例

*DataAccess_Basic*這份文件的範例程式碼看起來像這樣，在 iOS 上執行時。 程式碼說明如何執行簡單的 SQLite.NET 作業，並為應用程式的主視窗中的文字顯示結果。

**iOS**

 [![iOS SQLite.NET 範例](using-sqlite-orm-images/image2-sml.png)](using-sqlite-orm-images/image2-sml.png#lightbox)

下列程式碼範例顯示使用 SQLite.NET 程式庫來封裝基礎的資料庫存取整個資料庫互動。 它會顯示：

1.  建立資料庫檔案
1.  建立物件，然後將它們儲存插入一些資料
1.  查詢資料

您必須加入這些命名空間：

```csharp
using SQLite; // from the github SQLite.cs class
```

這需要您已新增至您的專案，以反白顯示 SQLite[此處](#Usage)。 請注意，SQLite 資料庫資料表藉由將屬性加入至類別定義 (`Stock`類別) 而不是 CREATE TABLE 命令。

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

使用`[Table]`屬性但未指定資料表名稱參數會導致基礎資料庫資料表有相同名稱做為類別 （在本例中為"Stock"）。 如果您撰寫 SQL 查詢，直接對資料庫而不使用 ORM 資料存取方法，是重要的實際資料表名稱。 同樣地`[Column("_id")]`屬性是選擇性的而且如果不存在的資料行會加入至具有相同名稱的類別中屬性的資料表。

## <a name="sqlite-attributes"></a>SQLite 屬性

您可以套用至您的類別，以控制基礎資料庫中的儲存方式的通用屬性包括：

-  **[PrimaryKey]** -這個屬性可以套用至整數屬性，以強制為基礎的資料表主索引鍵。 不支援複合主索引鍵。
-  **[自動遞增]** – 此屬性會為每個新物件插入至資料庫的自動遞增的整數屬性的值
-  **[Column(name)]** – 提供選擇性`name`參數會覆寫預設值的基礎資料庫資料行名稱 （這是與屬性相同）。
-  **[Table(name)]** – 將標示為基礎的 SQLite 資料表中儲存的類別。 指定選擇性的 name 參數，將會覆寫預設值的基礎資料庫資料表的名稱 （也就是類別名稱相同）。
-  **[MaxLength(value)]** – 嘗試進行資料庫的插入時，限制文字屬性的長度。 使用程式碼，應驗證此之前插入物件，這個屬性只有 '' 時檢查資料庫的插入或更新作業會嘗試。
-  **[忽略]** – 導致 SQLite.NET 忽略這個屬性。 這是特別適用於有無法儲存在資料庫中，類型的屬性或屬性無法自動解決的模型集合，是 SQLite。
-  **[Unique]** – 可確保基礎資料庫資料行中的值是唯一。


這些屬性大部分都是選擇性，SQLite 會使用預設值的資料表和資料行的名稱。 您應該一律指定為整數的主索引鍵，以便可以有效率地執行選取項目和刪除的查詢，對您的資料。

## <a name="more-complex-queries"></a>更複雜的查詢

上的下列方法`SQLiteConnection`可用來執行其他資料作業：

-  **插入**– 將新物件加入至資料庫。
-  **取得<T>** – 嘗試擷取使用的主索引鍵的物件。
-  **表格<T>** – 傳回資料表中的所有物件。
-  **刪除**– 刪除使用其主索引鍵的物件。
-  **查詢<T>** -執行 SQL 查詢會傳回 （當作物件） 的資料列數目。
-  **執行**– 使用這個方法 (而非`Query`) 時，您不想從 SQL （例如 INSERT、 UPDATE 和 DELETE 的指示） 的資料列。


### <a name="getting-an-object-by-the-primary-key"></a>取得主索引鍵的物件

SQLite.Net 提供 Get 方法來擷取單一物件，根據其主索引鍵。

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>選取物件使用 Linq

傳回集合的方法支援 IEnumerable<T>讓您可以使用 Linq 查詢，或排序資料表的內容。 下列程式碼示範如何使用 Linq 篩選出所有開頭為字母"A"的項目：

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>選取 使用 SQL 物件

即使 SQLite.Net 可以提供物件為基礎的存取，您的資料，有時您可能需要執行更複雜的查詢比 Linq 也允許 （或您可能需要更快的效能）。 如下所示，您可以使用查詢方法中，使用 SQL 命令：

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> 直接撰寫 SQL 陳述式時您會建立相依性的資料表和資料行在資料庫中，從您的類別和其屬性已產生的名稱。 如果您變更這些名稱在您的程式碼中，您必須記得更新任何手動編寫的 SQL 陳述式。

### <a name="deleting-an-object"></a>刪除物件

主索引鍵來刪除資料列，如下所示：

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

您可以檢查`rowcount`確認多少資料列受到影響 （在此情況下刪除）。

## <a name="using-sqlitenet-with-multiple-threads"></a>使用 SQLite.NET 具有多執行緒

SQLite 支援三種不同的執行緒模式：*單一執行緒*，*多執行緒*，並*序列化*。 如果您想要從沒有任何限制的多個執行緒存取的資料庫，您可以設定要使用 SQLite**序列化**執行緒模式。 請務必及早在您的應用程式中設定此模式 (例如，在開頭`OnCreate`方法)。

若要變更執行緒的模式，請呼叫`SqliteConnection.SetConfig`。 比方說，這行程式碼會設定適用於 SQLite**序列化**模式︰

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

## <a name="related-links"></a>相關連結

- [DataAccess Basic （範例）](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 進階 （範例）](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin.Forms 資料存取](~/xamarin-forms/app-fundamentals/databases.md)
