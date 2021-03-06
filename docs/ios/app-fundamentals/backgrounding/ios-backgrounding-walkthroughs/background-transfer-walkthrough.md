---
title: 背景傳送和 NSURLSession 在 Xamarin.iOS 中
description: 本文件提供的逐步解說，示範如何使用背景傳送和 NSUrlSession 來開始下載大型的映像中，並當應用程式都會在背景繼續下載。
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4e525388290d92901e68e61f1ffa81866f5aac4d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114232"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>背景傳送和 NSURLSession 在 Xamarin.iOS 中

藉由設定背景初始化背景傳輸`NSURLSession`和上傳或下載工作的佇列。 如果工作完成時背景、 暫止，或結束應用程式，iOS 會通知應用程式藉由在應用程式的呼叫完成處理常式*AppDelegate*。 下圖說明此動作：

 [![](background-transfer-walkthrough-images/transfer.png "背景傳送會藉由設定背景 NSURLSession 起始，並將上傳或下載工作")](background-transfer-walkthrough-images/transfer.png#lightbox)

我們來看看起來像這樣的程式碼中。

## <a name="configuring-a-background-session"></a>設定背景工作階段

若要讓背景工作階段，建立新`NSUrlSession`，並設定它使用`NSUrlSessionConfiguration`物件。

組態物件判斷工作階段可以做什麼，它可以執行的工作類型。
設定使用的工作階段`CreateBackgroundSessionConfiguration`方法會在不同的處理序中執行，並執行判別 (WiFi) 傳輸，以保留資料和電池壽命。
下列程式碼範例示範的背景傳輸工作階段使用適當設定`CreateBackgroundSessionConfiguration`方法和唯一字串識別項：

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

組態物件，除了工作階段也會需要工作階段代理人和佇列。
佇列會決定工作將會完成的順序。 工作階段代理人 chaperones 傳輸程序和處理驗證、 快取和其他工作階段相關的問題。

## <a name="working-with-tasks-and-delegates"></a>使用工作和委派

既然我們已經設定背景工作階段，讓我們開始處理傳輸的工作。 我們可以追蹤的使用這些工作`NSUrlSessionDelegate`執行個體稱為工作階段代理人。 工作階段代理人負責喚醒終止或暫停應用程式在背景中的處理驗證、 錯誤或傳輸完成。

`NSUrlSessionDelegate`提供下列基本的方法，若要檢查轉移狀態：

-  *DidFinishEventsForBackgroundSession* -所有工作都完成時，並傳送已完成時，會呼叫這個方法。
-  *DidReceiveChallenge* -要求認證需要授權時呼叫。
-  *DidBecomeInvalidWithError* -被呼叫的 if`NSURLSession`會變成無效。


背景工作階段需要根據執行的工作類型更特殊的委派。 背景工作階段僅限於兩種類型的工作項目：

-  *上傳工作*-類型的工作`NSUrlSessionUploadTask`使用`NSUrlSessionTaskDelegate`，該項則繼承自`NSUrlSessionDelegate`。 這個委派會提供額外的方法，來追蹤上傳進度、 控制代碼 HTTP 重新導向，和更多功能。
-  *下載工作*-類型的工作`NSUrlSessionDownloadTask`使用`NSUrlSessionDownloadDelegate`，該項則繼承自`NSUrlSessionTaskDelegate`。 這個委派會提供所有的方法上傳工作，以及下載特有的方法，來追蹤的下載進度，並判斷當下載工作繼續，或已完成。


下列程式碼會定義可用來從 URL 下載映像的工作。 我們開始進行工作，藉由呼叫`CreateDownloadTask`在我們的背景工作階段，並傳入 URL 要求：

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

接下來，我們會建立新的工作階段下載委派來追蹤所有下載工作在此工作階段中：

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

如果我們想要了解下載工作的進度，我們可以覆寫`DidWriteData`方法來追蹤進度，並甚至是更新 UI。 如果應用程式在前景，或將會等待使用者的下次開啟應用程式時，會立即出現 UI 更新。

工作階段代理人 API 會提供廣泛的工具組來與工作互動。 如需完整清單，工作階段的委派方法，請參閱`NSUrlSessionDelegate`API 文件。

> [!IMPORTANT]
> 背景工作階段並啟動背景執行緒，因此更新 UI 的任何呼叫必須明確地執行 UI 執行緒上呼叫`InvokeOnMainThread`以避免 iOS 終止應用程式。 


## <a name="handling-transfer-completion"></a>處理轉送完成

最後一個步驟是讓應用程式知道工作階段相關聯的所有工作都完成時，並處理新的內容。

在  *AppDelegate*，訂閱`HandleEventsForBackgroundUrl`事件。 當應用程式進入背景和傳輸工作階段正在執行時，會呼叫這個方法，而且系統會傳遞我們完成處理常式：

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

我們將使用完成處理常式，可讓 iOS 知道我們的應用程式完成時處理。

您應該記得在工作階段可以繁衍 （spawn） 處理傳輸的幾項工作。 最後一個工作完成時，暫止或終止的應用程式是在背景重新啟動。 然後，應用程式重新連接到`NSURLSession`使用唯一的工作階段識別項，並呼叫`DidFinishEventsForBackgroundSession`工作階段在委派上。 這個方法會處理新的內容，包括更新以反映的傳輸結果 UI 的應用程式的機會：

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

一旦我們完成時處理新的內容，我們會呼叫完成處理常式，讓系統知道它是安全的應用程式的快照，並回到睡眠狀態：

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

在本逐步解說中，我們會涵蓋實作背景傳送服務在 iOS 7 中的基本步驟。



## <a name="related-links"></a>相關連結

- [簡單背景傳送 （範例）](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
