---
title: 即時重新載入
description: 請參閱變更您的 XAML 反映即時，而不需要其他的編譯和部署。
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: ca359e5ea700ef09249a2d8a299b6604f91e9149
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="xamarin-live-reload"></a>Xamarin 即時重新載入

![預覽](~/media/shared/preview.png)

Xamarin 即時重新載入可讓您**變更 XAML 看到其反映即時，而不需要其他的編譯和部署**。 XAML 所做的變更將會重新部署上儲存，並反映在您部署的目標。

使用即時重新載入時，會編譯您的應用程式，因為它可搭配所有媒體櫃和協力廠商控制項。 Xamarin.Forms 支援，包括 Android、 iOS 和 UWP，以及適用於所有有效的部署目標，包括模擬器，模擬器，以及實體裝置上的所有平台上的即時重新載入運作。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

即時重新載入目前僅提供的 Visual Studio 2017 中。

## <a name="requirements"></a>需求

* [Visual Studio 2017 15.7 版本或更高版本](https://www.visualstudio.com/vs/)或更新版本與**行動應用程式開發的.NET**工作負載。
* [Xamarin.Forms 3.0.0 以上](https://www.nuget.org/packages/Xamarin.Forms/)或更新版本。

## <a name="getting-started"></a>快速入門
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1.安裝 Xamarin 即時重新載入，從 Visual Studio Marketplace

透過 Visual Studio Marketplace 散發 Xamarin 即時重新載入。 若要安裝擴充功能，請造訪[Xamarin 即時重新載入頁面，在 Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload)網站，並按一下**下載**。

開啟.vsix，下載之後，按一下**安裝**。

![Visual Studio 安裝程式 Xamarin 即時重新載入確認](images/LiveReloadVSIXInstall.png)

或者，您可以搜尋中**線上**索引標籤中**擴充功能和更新**在 Visual Studio 內的對話方塊。

### <a name="2-configure-your-app-to-use-live-reload"></a>2.設定應用程式以使用即時重新載入

即時重新載入現有的行動裝置應用程式可新增三個步驟：

1. 請確認所有專案都會都更新為使用[Xamarin.Forms 3.0.0 以上](https://www.nuget.org/packages/Xamarin.Forms/)或更新版本。

2. 新增**Xamarin.LiveReload** NuGet 封裝：

    a. **標準.NET** – 安裝**Xamarin.LiveReload** NuGet 到您的.NET 標準 2.0 程式庫。 這不需要安裝在您的平台專案。 請確認**套件來源**設**所有**。
    
    b. **共用的專案**– 安裝**Xamarin.LiveReload** NuGet 在所有平台專案 （例如 Android、 iOS、 UWP，等）。 請確認**套件來源**設**所有**。

    [![新增 Xamarin 即時重新載入 NuGet 使用 NuGet 封裝管理員](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. 新增`LiveReload.Init();`建構函式中`Application`類別，如下列程式碼片段所示：

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3.啟動即時重新載入

編譯及部署您的應用程式。 已部署應用程式之後，開啟 XAML 檔案，進行一些變更，然後儲存檔案。 部署目標來重新部署您的變更。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

即時重新載入適用於任何 XAML 檔案的變更。 C# 或加入/移除 NuGet 套件的變更需要新的組建，並部署至才會生效。

## <a name="frequently-asked-questions"></a>常見問題集 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>位於 Xamarin 即時重新載入 Visual Studio for Mac 嗎？ 

只有適用於 Visual Studio 2017 Xamarin 即時重新載入初始的預覽版本。 Visual Studio for Mac 支援已規劃在未來的版本。

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>這與所有的程式庫，例如角柱運作？ 

因為您的應用程式編譯時，所有程式庫，例如角柱和協力廠商控制項程式庫，例如 Telerik、 Infragistics、 Syncfusion、 ArcGIS、 GrapeCity 和其他控制項廠商適用於即時重新載入。

### <a name="what-changes-does-live-reload-redeploy"></a>即時重新載入部署了哪些變更？ 

即時重新載入只適用於 XAML 或 CSS 所做的變更。 如果您以 C# 檔案中進行變更，將會需要重新編譯。 重新載入 C# 的支援已規劃在未來的版本。

### <a name="what-platforms-are-supported"></a>支援哪些平台？ 

透過 Xamarin.Forms，包括 Android、 iOS 和 UWP 所支援的任何平台上的即時重新載入運作。

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>這在模擬器，模擬器和實體裝置上運作？ 

[是]，即時重新載入搭配運作，包括 Android 模擬器、 iOS 模擬器和實體裝置的所有有效的部署目標。 部署至裝置需要裝置與電腦位於相同的 Wi-fi 網路。

### <a name="does-this-work-with-corporate-networks"></a>這與公司網路運作？

如果您在偵錯 Android 模擬器或 iOS 模擬器，即時重新載入會使用 localhost 來通訊。 如果您想要部署到裝置時，必須在相同的 Wi-fi 網路裝置和電腦。 您可以在其中不可行的情況下，[設定即時重新載入伺服器](#live-reload-server)，這會讓您即時重新載入，不論網路連線設定。

### <a name="does-it-require-debugging-the-app"></a>它需要偵錯應用程式嗎？ 

否。 事實上，您甚至可以任意數目的裝置或模擬器/模擬器上啟動所有您支援的應用程式的目標 （Android、 iOS 和 UWP），並查看全部一次更新。 

## <a name="limitations"></a>限制

* 只重新載入的 XAML 支援。
* 除非使用 MVVM UI 狀態可能無法維護之間重新部署。

## <a name="known-issues"></a>已知問題

* 只支援 Visual Studio 中。
* 重新載入整個應用程式的資源 (也就是**App.xaml**或共用資源字典)，就會重設應用程式瀏覽。 這將在下一步的預覽版本中修正。
* 當偵錯 UWP 可能會造成執行階段當機，請編輯 XAML。 因應措施： 使用**啟動但不偵錯 （Ctrl + F5）** 而不是**開始偵錯 (F5)**。

## <a name="troubleshooting"></a>疑難排解

### <a name="error-codes"></a>錯誤碼

* **XLR001**:*目前的專案參考 'Xamarin.LiveReload' NuGet 套件版本 [版本]，但 Xamarin 即時重新載入延伸模組需要版本 [版本]。*

  為了讓快速反覆項目和即時重新載入功能的演進，必須完全符合 nuget 套件和 Visual Studio 擴充功能。 為相同的版本，您已安裝的擴充功能的更新您的 nuget 套件。

* **XLR002**:*即時重新載入至少需要 'MqttHostname' 屬性從命令列建置時。或者，設定 'EnableLiveReload' 為 'false' 停用此功能。*

  即時重新載入所需的屬性時，就無法使用從命令列 （或連續整合中），建置，因此必須明確提供。 

* **XLR003**:*即時重新載入 nuget 套件需要安裝 Xamarin 即時重新載入 Visual Studio 擴充功能。*

  嘗試建置專案，參考即時重新載入 nuget 封裝，但未安裝 Visual 的擴充功能。  

### <a name="app-doesnt-connect"></a>應用程式並不會連接

當建置應用程式，從資訊**工具 > 選項 > Xamarin > 即時重新載入**（主機名稱、 連接埠和加密金鑰） 會因此內嵌在應用程式中，當`LiveReload.Init()`沒有配對或組態執行是需要連接才會成功。

非標準網路問題 （防火牆，在不同的網路上的裝置），應用程式可能無法順利連線 IDE 的主要原因是因為其設定與 Visual Studio 中的一個。 如果此情形：

* 一部電腦上編譯應用程式。
* 應用程式已編譯和部署在不同的 Visual Studio 工作階段，並**自動產生加密金鑰**是簽入的 （預設值）**工具 > 選項 > Xamarin > 即時重新載入**。
* Visual Studio 設定已變更 （也就是主機名稱、 連接埠或加密金鑰），但未建置並重新部署應用程式。

這些情況下會建置並重新部署應用程式解決。

### <a name="uninstalling-preview-1"></a>解除安裝 Preview 1

如果您有較舊的預覽，並解除安裝的問題，請遵循下列步驟：

1. 刪除資料夾**C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (注意： 您已安裝的版本，以取代"Enterprise"和"Preview"與"2017 「 如果您安裝到穩定 VS）
2. 開啟**開發人員命令提示字元**該 Visual Studio 和執行`devenv /updateconfiguration`。 

## <a name="tips--tricks"></a>秘訣和訣竅

* 只要即時重新載入設定不會變更 (包括加密金鑰，例如，如果您關閉**自動產生加密金鑰**) 和您建立從同一部電腦，您不需要建置和部署應用程式之後部署，除非您變更程式碼或相依性。 您可以只再次啟動先前部署的應用程式和它的最後一個會連接使用的主機。

* 會有多少裝置，您可以連接到相同的 Visual Studio 工作階段上沒有限制。 您可以部署，並視需要，請參閱即時 reloading 上全部都在相同的時間，最多的裝置/模擬器中啟動應用程式。

* 即時重新載入只會重新載入您的應用程式的使用者介面部分，但也不會*不*重新建立您的網頁，都不它取代您檢視模型 （或繫結內容）。 這表示*整個*應用程式狀態一定會保留在重新載入，包括您插入的相依性。

## <a name="live-reload-server"></a>即時重新載入伺服器

在案例中從執行中應用程式連接到您的電腦 (表示使用`localhost`或`127.0.0.1`中**工具 > 選項 > Xamarin > 即時重新載入**) 無法 （也就是防火牆，不同的網路）您可以將遠端伺服器設定相反地，IDE 和應用程式將以連現到。

即時重新載入使用標準[MQTT 通訊協定](http://mqtt.org/)來交換訊息，並因此可以與[協力廠商伺服器](https://github.com/mqtt/mqtt.github.io/wiki/servers)。 即使有[公用伺服器](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers)(也稱為*broker*) 可用，您可以使用。 即時重新載入經過測試可與`broker.hivemq.com`和`iot.eclipse.org`主機名稱，以及所提供的服務[www.cloudmqtt.com](https://www.cloudmqtt.com)和[www.cloudamqp.com](https://www.cloudamqp.com)。您也可以如部署在雲端中，您自己 MQTT 伺服器[在 Azure 上的 HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)。

您可以設定任何連接埠，但通常會使用預設 1883年連接埠使用遠端伺服器。 即時重新載入訊息使用強式端對端 AES 對稱式加密，因此安全地連線至遠端伺服器。 根據預設，加密金鑰和初始化向量 (IV) 會重新產生每個 Visual Studio 工作階段上。

可能的最簡單方式是安裝[mosquitto](https://mosquitto.org)空白 Ubuntu VM 在 Azure 中的伺服器：

1. 在 Azure 入口網站中建立新的 Ubuntu Server VM
2. 針對 1883 （預設 MQTT 連接埠） 在 [網路] 索引標籤中加入新的輸入連接埠規則
3. 開啟[雲端殼層](https://docs.microsoft.com/azure/cloud-shell/overview)（撞模式）
4. 輸入`ssh [USERNAME]@[PUBLIC_IP]`使用您選擇 1 中的使用者名稱) 與 VM 的 [概觀] 頁面中顯示的公用 IP
5. 執行`sudo apt-get install mosquitto`，輸入您在 1 中選擇的密碼)

現在您可以使用該 IP 連接到您自己 MQTT 伺服器。