---
title: 自訂在 Xamarin.iOS 中的資料表的外觀
description: 本文件說明如何自訂在 Xamarin.iOS 中的資料表的外觀。 它討論的儲存格樣式、 附屬應用程式、 資料格分隔符號，以及自訂的資料格的版面配置。
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0282b4b2194411d503ef7eb54b0337272e2be3ed
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121317"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>自訂在 Xamarin.iOS 中的資料表的外觀

變更資料表的外觀的最簡單方式是使用不同的儲存格樣式。 您可以建立每個儲存格時，會使用哪一個儲存格樣式來變更`UITableViewSource`的`GetCell`方法。

## <a name="cell-styles"></a>儲存格樣式

有四個內建樣式：

-  **預設值**– 支援`UIImageView`。
-  **翻譯字幕**– 支援`UIImageView`和副標題。
-  **Value1** -正確對齊的子標題，支援`UIImageView`。
-  **Value2** – 標題會靠右對齊和子標題靠左對齊 （但沒有映像）。


這些螢幕擷取畫面顯示每種樣式的顯示方式：

 [![](customizing-table-appearance-images/image7.png "這些螢幕擷取畫面顯示每種樣式的顯示方式")](customizing-table-appearance-images/image7.png#lightbox)

此範例**CellDefaultTable**包含程式碼來產生這些畫面。 在中設定的儲存格樣式`UITableViewCell`建構函式，就像這樣：

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[支援的屬性](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/)儲存格樣式可以設定：

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>附屬應用程式

資料格可以包含檢視的右邊加入下列附屬應用程式：

-   **核取記號**– 可用來指出多重選取資料表中。
-   **DetailButton** – 回應觸控與其餘的資料格，讓它可以執行不同的函式，若要碰觸到儲存格本身無關 (例如開啟快顯視窗或新的視窗不屬於`UINavigationController`堆疊)。
-   **DisclosureIndicator** ： 通常用來指出觸及的資料格就會開啟另一個檢視。
-   **DetailDisclosureButton** – 組成`DetailButton`和`DisclosureIndicator`。


這是什麼樣子：

 [![](customizing-table-appearance-images/image8.png "範例 附屬應用程式")](customizing-table-appearance-images/image8.png#lightbox)

若要顯示這些 附屬應用程式，您可以設定的其中一個`Accessory`屬性中的`GetCell`方法：

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

當`DetailButton`或是`DetailDisclosureButton`會顯示，您也應該覆寫`AccessoryButtonTapped`時都會被接觸到執行某些動作。

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

此範例**CellAccessoryTable**示範如何使用 附屬應用程式。

## <a name="cell-separators"></a>資料格分隔符號

資料格分隔符號是用來分隔資料表的資料表資料格。 在資料表上所設定的屬性。

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

此外，也可以加入分隔符號柔邊或 vibrancy 效果：

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

分隔符號也可以嵌入：

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>建立自訂儲存格的版面配置

若要變更之視覺樣式的資料表，您必須提供自訂的資料格，才顯示。 自訂儲存格可以有不同的色彩和控制項版面配置。

CellCustomTable 範例會實作`UITableViewCell`定義的自訂版面配置的子類別`UILabel`s 和`UIImage`與不同的字型和色彩。 產生的資料格看起來像這樣：

 [![](customizing-table-appearance-images/image9.png "自訂儲存格的版面配置")](customizing-table-appearance-images/image9.png#lightbox)

自訂儲存格類別包含三個方法：

-   **建構函式**– 建立 UI 控制項，並設定自訂的樣式屬性 （例如。 字體、 大小和色彩）。
-   **UpdateCell** – 一種方法`UITableView.GetCell`用來設定儲存格的屬性。
-   **LayoutSubviews** – 設定 UI 控制項的位置。 在範例中，每個資料格具有相同的配置，但更複雜的資料格 （尤其是具有不同大小） 可能需要不同的版面配置位置，視要顯示的內容而定。


中的完整範例程式碼**CellCustomTable > CustomVegeCell.cs**遵循：

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell`方法的`UITableViewSource`需要加以修改才能建立自訂的資料格：

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>相關連結

- [WorkingWithTables （範例）](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
