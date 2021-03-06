---
ms.openlocfilehash: 54bee14f6de409550357194e905b6ee5d8ef8f18
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2020
ms.locfileid: "90678991"
---
### <a name="outputtype-set-to-winexe-for-wpf-and-winforms-apps"></a>適用于 WPF 和 WinForms 應用程式的 OutputType 設定為 WinExe

`OutputType` 會自動設為， `WinExe` Windows Presentation Foundation (WPF) 和 Windows Forms 應用程式。 當 `OutputType` 設定為時 `WinExe` ，在執行應用程式時，不會開啟主控台視窗。

#### <a name="change-description"></a>變更描述

在舊版的 .NET 中，會使用專案檔中所指定的值 `OutputType` 。 例如：

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

從 .NET 5.0 開始， `OutputType` 會 `WinExe` 針對 WPF 和 Windows Forms 的應用程式自動設定為。 例如：

```xml
<PropertyGroup>
  <OutputType>WinExe</OutputType>
</PropertyGroup>
```

#### <a name="reason-for-change"></a>變更的原因

假設在執行 WPF 或 Windows Forms 應用程式時，大部分的使用者不希望主控台視窗開啟。 此外， [現在這些應用程式類型會使用 .NET sdk](../../../../docs/core/compatibility/wpf.md#winforms-and-wpf-apps-use-microsoftnetsdk) ，而不是使用 WINDOWS 桌面 sdk，因此會設定正確的預設值。 此外，當新增以 iOS 和 Android 為目標的支援時，如果在多個平臺上都使用相同的輸出類型，就會比較容易使用多個平臺。

#### <a name="version-introduced"></a>引進的版本

.NET 5.0 Preview 8

#### <a name="recommended-action"></a>建議的動作

在您的部分中不需要採取任何動作。 但是，如果您想要還原為舊的行為，請 `DisableWinExeOutputInference` 在專案檔中將屬性設為 `true` 。

```xml
<DisableWinExeOutputInference>true</DisableWinExeOutputInference>
```

#### <a name="category"></a>類別

- Windows Forms
- Windows Presentation Framework (WPF)

#### <a name="affected-apis"></a>受影響的 API

無法透過 API 分析偵測。

<!-- 

#### Affected APIs

Not detectable via API analysis.

-->
