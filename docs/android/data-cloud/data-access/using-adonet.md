---
title: "使用 ADO.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 6c5b01f30bf2bbdf7ad8a7fbca2500b220a38d01
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
---
# <a name="using-adonet"></a>使用 ADO.NET

Xamarin 具有內建支援可在 Android 上且可以使用熟悉的 ADO.NET 類似語法公開 SQLite 資料庫。 使用這些 Api 會要求您撰寫 SQL 陳述式所 SQLite，例如處理`CREATE TABLE`，`INSERT`和`SELECT`陳述式。

## <a name="assembly-references"></a>組件參考

若要使用存取透過 ADO.NET，您必須新增 SQLite`System.Data`和`Mono.Data.Sqlite`參考至您的 Android 專案，如下所示：

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![在 Visual Studio 中的 android 參考](using-adonet-images/image7.png "Android 參考 Visual Studio 中") 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac) 

![在 Visual Studio for Mac 的 android 參考](using-adonet-images/image5.png "Android 參考在 Visual Studio for Mac") 

-----


以滑鼠右鍵按一下**參考 > 編輯參考...** ，然後按一下以選取必要的組件。

## <a name="about-monodatasqlite"></a>關於 Mono.Data.Sqlite

我們將使用`Mono.Data.Sqlite.SqliteConnection`類別來建立空白的資料庫檔案然後再執行個體化`SqliteCommand`我們可以使用來執行對資料庫的 SQL 指令的物件。

**建立空白資料庫**&ndash;呼叫`CreateFile`方法的有效 (ie。 可寫入) 檔案路徑。 您應該檢查是否檔案已經存在之前呼叫這個方法，透過頂端的舊密碼，否則會建立新的 （空白） 資料庫和舊的檔案中的資料將會遺失。
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath`變數應該根據這份文件稍早所述的規則決定。

**建立資料庫連接** &ndash; SQLite 資料庫檔案建立之後，您就可以建立連接物件來存取資料。 連接的連接字串，它使用的格式，建構`Data Source=file_path`，如下所示：

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

如先前所述，連接也不應重複使用多個不同的執行緒。 不確定，建立所需連接並且將它關閉，當您完成時;但請留意這個超過通常太需要執行這項。

**建立和執行資料庫命令**&ndash;一旦連接我們可以執行任意的 SQL 命令，對它。 程式碼所示`CREATE TABLE`正在執行陳述式。

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

直接對資料庫執行 SQL 時，您應該採取正常的預防措施，不以提出無效的項目，例如嘗試建立已存在的資料表。 追蹤的資料庫的結構，因此不會導致`SqliteException`例如**SQLite 錯誤資料表 [項目] 已經存在**。

## <a name="basic-data-access"></a>基本的資料存取

*DataAccess_Basic*時在 Android 上執行這份文件的範例程式碼看起來像這樣：

![Android ADO.NET 範例](using-adonet-images/image8.png "Android ADO.NET 範例")

下列程式碼說明如何執行簡單的 SQLite 作業，並為應用程式的主視窗中的文字會顯示在結果。

您必須加入這些命名空間：

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

下列程式碼範例會顯示整個資料庫互動：

1.  建立資料庫檔案
2.  插入一些資料
3.  查詢資料

這些作業會通常會出現在多個位置，在整個程式碼，例如您可能建立的資料庫檔案和資料表，您的應用程式初次啟動時，並在您的應用程式中執行資料讀取和寫入個別的畫面中。 在下列範例中均已分組為此範例中的單一方法：

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>更複雜的查詢

因為 SQLite 允許任意的 SQL 命令，以針對資料執行，所以您可以執行任何`CREATE`， `INSERT`， `UPDATE`， `DELETE`，或`SELECT`您喜歡的陳述式。 您可以閱讀 SQLite 支援 Sqlite 網站上的 SQL 命令。 執行 SQL 陳述式上使用三種方法之一`SqliteCommand`物件：

-   **ExecuteNonQuery** &ndash;通常用於資料表建立或資料插入。 某些作業的傳回值的受影響的資料列數目，否則它便是-1。

-   **ExecuteReader** &ndash;應該做為傳回的資料列集合時使用`SqlDataReader`。

-   **ExecuteScalar** &ndash;擷取單一值 （例如彙總）。


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT``UPDATE`，和`DELETE`陳述式會傳回受影響的資料列數目。 所有其他 SQL 陳述式會傳回-1。

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

下列方法示範`WHERE`中的子句`SELECT`陳述式。
程式碼製作完整的 SQL 陳述式，因為它必須謹慎地逸出保留的字元，例如字串周圍的引號 （'）。

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

`ExecuteReader` 方法會傳回 `SqliteDataReader` 物件。 除了`Read`方法範例中顯示其他有用的屬性包括：

-   **RowsAffected** &ndash;查詢所影響的資料列計數。

-   **HasRows** &ndash;是否傳回任何資料列。


### <a name="executescalar"></a>EXECUTESCALAR

使用此作業對於`SELECT`傳回單一值 （例如彙總） 的陳述式。

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar`方法的傳回型別是`object`&ndash;您應該根據資料庫查詢結果的轉換。 結果可能會從整數`COUNT`查詢或單一資料行的字串`SELECT`查詢。 請注意，這不同於其他`Execute`傳回讀取器物件或影響的資料列數的方法。



## <a name="related-links"></a>相關連結

- [DataAccess Basic （範例）](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [資料存取進階 （範例）](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android 資料配方](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms 資料存取](~/xamarin-forms/app-fundamentals/databases.md)