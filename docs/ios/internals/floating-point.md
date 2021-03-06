---
title: 在 Xamarin.iOS 中浮點數作業
description: 本文件說明 Xamarin.iOS 的 32 位元和 64 位元精確度浮點數作業的處理方式，並討論相關聯的影響效能。
ms.prod: xamarin
ms.assetid: 003F25C1-B430-4339-9C95-7DF527EBC699
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: c5ee1b833e309c78c7338298cbe5c8800afb1ba1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116234"
---
# <a name="floating-point-operations-in-xamarinios"></a>在 Xamarin.iOS 中浮點數作業

Xamarin.iOS 預設會執行 32 位元和 64 位元浮點作業在 ARM 上使用 64 位元精確度。  

開發人員預期從浮點數作業，在更接近這個較高的有效位數時C#在桌面上，行動裝置的效能影響可能更為顯著。

就可以編譯您 32 位元浮點點程式碼以使用 32 位元浮點運算。  若要這樣做，您需要至少使用 Xamarin.iOS 8.10 和設定您的 iOS 上建置選項的面板"mtouch 多餘的引數 」 項目行的下列值：

     --aot-options=-O=float32

這會通知靜態編譯器 （Mono 的靜態編譯器內建或 LLVM 提供一個） 來執行使用 32 位元浮點數的浮點數作業。
