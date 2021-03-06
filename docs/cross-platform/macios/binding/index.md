---
title: 繫結 Objective C
description: 這份文件會提供連結，說明如何建立 C# 的繫結 OBJECTIVE-C 程式碼，讓開發人員可以使用現成的程式庫，在 Xamarin 應用程式中的各種輔助線。
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855152"
---
# <a name="binding-objective-c"></a>繫結 Objective C

本節包含各種不同的文件，其中涵蓋建立繫結 Objective C 程式庫，讓他們可以從 C# 建立的 Xamarin.iOS 或 Xamarin.Mac 應用程式呼叫。

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[概觀](~/cross-platform/macios/binding/overview.md)

本文件包含一些內部如何繫結會發生。 它是進階的文件的某些技術的資訊。

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[繫結 Objective-C 程式庫](~/cross-platform/macios/binding/objective-c-libraries.md)

本文件說明用來建立 C# 的 Objective C Api 以及如何在 OBJECTIVE-C 慣用語會對應至在.NET 中使用的慣用語的繫結程序。
如果您要繫結只 C Api，您應該使用標準的.NET 機制，為此，P/Invoke framework。

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[繫結定義參考指南](~/cross-platform/macios/binding/binding-types-reference.md)

這是描述所有繫結至磁碟機繫結產生程序的作者可以使用的屬性參考指南。


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標 Sharpie 是協助啟動程序繫結的第一個階段的命令列工具。 它藉由剖析對應到公用 API 的原生程式庫的標頭檔的運作方式[繫結定義](~/cross-platform/macios/binding/objective-c-libraries.md)（也可以手動完成這個程序）。

## <a name="ios"></a>iOS

[IOS 繫結頁面](~/ios/platform/binding-objective-c/index.md)連結回這些一般的繫結資源，除了下列範例。

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[逐步解說： 繫結 Objective C 程式庫](~/ios/platform/binding-objective-c/walkthrough.md)

這篇文章提供建立繫結專案使用開放原始碼的逐步解說[InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective C 專案，做為範例。 InfColorPicker 程式庫提供可重複使用檢視控制器可讓使用者選取根據其 HSB 表示法，讓使用者更容易使用的色彩選取範圍的色彩。 目標 Sharpie 將用來協助進行繫結程序中。

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[繫結範例](https://github.com/mono/monotouch-bindings)

建立新的繫結的專案時，可以是第三方繫結的集合會使用參考。

## <a name="mac"></a>Mac

在過去[Mac 繫結](~/mac/platform/binding.md)已經非常手動程序。 目前沒有[可下載預覽](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview)的 Mac 繫結專案支援未來的版本推出的 Visual Studio for mac。



## <a name="related-links"></a>相關連結

- [iOS 繫結](~/ios/platform/binding-objective-c/index.md)
- [Mac 繫結](~/mac/platform/binding.md)
- [Xamarin University 課程： 建置 OBJECTIVE-C 繫結程式庫](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 課程： 建置目標 Sharpie OBJECTIVE-C 繫結程式庫](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
