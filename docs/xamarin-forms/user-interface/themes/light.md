---
title: "淺色佈景主題"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 21d5478c7e8317510a6f46b72a261582c9432301
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2018
---
# <a name="light-theme"></a>淺色佈景主題

![](~/media/shared/preview.png "這個 API 目前處於預覽狀態")

> [!NOTE]
> 佈景主題需要 Xamarin.Forms 2.3 預覽版本。 請檢查[疑難排解提示](~/xamarin-forms/user-interface/themes/index.md)如果發生錯誤。

若要使用淺色佈景主題：

## <a name="1-add-nuget-packages"></a>1.新增 Nuget 封裝

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2.加入資源字典

在**App.xaml**檔案新增新的自訂`xmlns`佈景主題，然後確定 佈景主題的資源會合併與應用程式的資源字典。
XAML 檔案的範例如下所示：

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3.載入佈景主題類別

後面要接著[疑難排解步驟](~/xamarin-forms/user-interface/themes/index.md)並新增必要的程式碼中的 iOS 和 Android 應用程式專案。

## <a name="4-use-styleclass"></a>4.使用 StyleClass

以下是範例的按鈕和淺色佈景主題，以及產生它們的標記中的標籤。

[ ![](light-images/light-theme-sml.png "按鈕和淺色佈景主題標籤")](light-images/light-theme.png "按鈕和標籤淺色佈景主題")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />
    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

[內建類別的完整清單](~/xamarin-forms/user-interface/themes/index.md)顯示何種樣式的一些常見的控制項可用。
