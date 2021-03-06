---
title: Xamarin.Forms 資料範本
description: DataTemplate 用於在支援的控制項中指定資料外觀，通常會繫結至要顯示的資料。
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 771ae22c3e28a4fce758bbfd6a3bd63bafb75e53
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994967"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms 資料範本

_DataTemplate 用於在支援的控制項中指定資料外觀，通常會繫結至要顯示的資料。_

## <a name="introductionintroductionmd"></a>[簡介](introduction.md)

Xamarin.Forms 資料範本可以在支援的控制項上定義資料呈現方式。 本文提供資料範本的簡介，探討為何資料範本是必要的。

## <a name="creating-a-datatemplatecreatingmd"></a>[建立 DataTemplate](creating.md)

您可以在 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 中，或是從自訂類型或適當的 Xamarin.Forms 資料格類型，以內嵌方式建立資料範本。 如果不需要在他處重複使用資料範本，則應該使用內嵌範本。 或者，您可以將資料範本定義為自訂類型，或是定義為控制項層級、頁面層級或應用程式層級資源，藉此重複使用資料範本。

## <a name="creating-a-datatemplateselectorselectormd"></a>[建立 DataTemplateSelector](selector.md)

在執行階段，您可以根據資料繫結屬性值，使用 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) 來選擇 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)。 這可將多個 `DataTemplate` 執行個體套用至相同類型的物件，以自訂特定物件的外觀。 本文示範如何建立和使用 `DataTemplateSelector`。


## <a name="related-links"></a>相關連結

- [資料範本 (範例)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
