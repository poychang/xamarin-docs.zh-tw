---
title: "使用者設定檔"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>使用者設定檔

Android 已支援的列舉連絡人`ContactsContract`自應用程式開發介面層級 5 提供者。 例如，若要清單連絡人很簡單，只使用`ContactContracts.Contacts`類別，如下列程式碼所示：

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Android 4 (API 層級 14) 的新`ContactsContact.Profile`類別是可透過 ContactsContract 提供者。 `ContactsContact.Profile`可存取個人的設定檔之裝置，包括裝置擁有者的名稱和電話號碼等連絡資料擁有者。

<a name="Required_Permissions" />

## <a name="required-permissions"></a>必要的權限

若要讀取和寫入連絡資料，應用程式必須要求`Read_Contacts`和`Write_Contacts`權限，分別。 此外，若要閱讀和編輯使用者設定檔，應用程式必須要求`Read_Profile`和`Write_Profile`權限。

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>更新設定檔資料

一旦已設定這些權限，應用程式可以使用 Android 的一般技術與使用者設定檔的資料互動。 例如，若要更新的設定檔的顯示名稱，我們會呼叫`ContentResolver.Update`與`Uri`透過擷取`ContactsContract.Profile.ContentRawContactsUri`屬性，如下所示：

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>讀取設定檔資料

發出查詢，以便`ContactsContact.Profile.ContentUri`讀取備份設定檔資料。 例如，下列程式碼會讀取使用者設定檔的顯示名稱：

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>瀏覽至使用者應用程式

最後，若要瀏覽至新的人員應用程式隨附 Android 4 中的使用者設定檔，只要建立與意圖`ActionView`動作和`ContactsContract.Profile.ContentUri`，並將它傳遞給`StartActivity`方法如下：

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

當執行上述程式碼，人員應用程式會載入使用者設定檔，如下列螢幕擷取畫面所示：

[![人員的螢幕擷取畫面顯示 John Doe 的使用者設定檔的應用程式](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

使用使用者設定檔現在是類似於與 Android 中的其他資料互動，而且提供一層額外的裝置個人化。



## <a name="related-links"></a>相關連結

- [ContactsProviderDemo (sample)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [介紹的冰淇淋三明治](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 平台](http://developer.android.com/sdk/android-4.0.html)