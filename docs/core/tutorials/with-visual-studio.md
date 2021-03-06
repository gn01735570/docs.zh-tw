---
title: 使用 Visual Studio 建立 .NET Core 主控台應用程式
description: '瞭解如何使用 Visual Studio 以 c # 或 Visual Basic 來建立 .NET Core 主控台應用程式。'
ms.date: 06/08/2020
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet
ms.openlocfilehash: d543a05eb00a59c5c08ada28fc8392875385aa8a
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537531"
---
# <a name="tutorial-create-a-net-core-console-application-using-visual-studio"></a>教學課程：使用 Visual Studio 建立 .NET Core 主控台應用程式

本教學課程說明如何在 Visual Studio 2019 中建立和執行 .NET Core 主控台應用程式。

## <a name="prerequisites"></a>必要條件

- 已安裝 **.Net Core 跨平臺開發**工作負載的[Visual Studio 2019 16.6 版或更新版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。 當您選取此工作負載時，會自動安裝 .NET Core 3.1 SDK。

  如需詳細資訊，請參閱 [使用 Visual Studio 安裝 .NET Core SDK](../install/windows.md#install-with-visual-studio)。

## <a name="create-the-app"></a>建立應用程式

建立名為 "HelloWorld" 的 .NET Core 主控台應用程式專案。

1. 啟動 Visual Studio 2019。

1. 在 [開始] 頁面上，選擇 [ **建立新專案**]。

   ![在 Visual Studio 開始] 頁面上選取 [建立新的專案] 按鈕](./media/with-visual-studio/start-window.png)

1. 在 [ **建立新專案** ] 頁面的 [搜尋] 方塊中，輸入 **主控台** 。 接著，選擇 [語言] 清單中的 [ **c #** ] 或 [ **Visual Basic** ]，然後從 [平臺] 清單中選擇 [ **所有平臺** ]。 選擇 ** ( .Net Core) 範本的主控台應用程式 ** ，然後選擇 [ **下一步]**。

   ![使用選取的篩選準則建立新的專案視窗](./media/with-visual-studio/create-new-project.png)

   > [!TIP]
   > 如果您看不到 .NET Core 範本，可能是缺少必要的工作負載。 在 [ **找不到您要尋找的專案嗎？** ] 訊息中，選擇 [ **安裝更多工具和功能** ] 連結。 Visual Studio 安裝程式隨即開啟。 請確定您已安裝 **.Net Core 跨平臺開發** 工作負載。

1. 在 [**設定您的新專案**] 對話方塊中，于 [**專案名稱**] 方塊中輸入**HelloWorld** 。 接著，選擇 [建立]  。

   ![使用 [專案名稱]、[位置] 和 [方案名稱] 欄位設定新的專案視窗](./media/with-visual-studio/configure-new-project.png)

此範本會建立簡單的 "Hello World" 應用程式。 它會呼叫 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法來顯示 "Hello World！" 。

範本 `Program` 程式碼會使用單一方法定義類別， `Main` 這個方法會採用 <xref:System.String> 陣列作為引數：

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

```vb
Imports System

Module Program
    Sub Main(args As String())
        Console.WriteLine("Hello World!")
    End Sub
End Module
```

`Main` 是應用程式進入點，是執行階段在啟動應用程式時會自動呼叫的方法。 在應用程式啟動時所提供的所有命令列引數，都會在 *args* 陣列中提供。

如果未顯示您想要使用的語言，請變更頁面頂端的語言選取器。

## <a name="run-the-app"></a>執行應用程式

1. 按下<kbd>Ctrl</kbd> + <kbd>F5</kbd>以執行程式，而不進行偵錯工具。

   主控台視窗隨即開啟，並出現 "Hello World！" 文字 列印在畫面上，而某些 Visual Studio 的 debug 資訊。

   ![主控台視窗顯示 Hello World，請按任意鍵以繼續](./media/with-visual-studio/hello-world-console.png)

1. 按任意鍵關閉主控台視窗。

## <a name="enhance-the-app"></a>增強應用程式

增強應用程式以提示使用者輸入其名稱，並將它與日期和時間一起顯示。

1. 在 *Program.cs* 或 *Program*中，以 `Main` 下列程式碼取代方法的內容，也就是呼叫的行 `Console.WriteLine` ：

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::
   :::code language="vb" source="./snippets/with-visual-studio/vb/Program.vb" id="MainMethod":::

   這段程式碼會在主控台視窗中顯示提示，並等候使用者輸入字串，然後按 <kbd>Enter</kbd> 鍵。 它會將此字串儲存在名為的變數中 `name` 。 它也會抓取 <xref:System.DateTime.Now?displayProperty=nameWithType> 屬性值，其中包含目前的當地時間，並將它指派給 `date` Visual Basic) 中名為 (的變數 `currentDate` 。 它會在主控台視窗中顯示這些值。 最後，它會在主控台視窗中顯示提示，並呼叫 <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> 方法來等候使用者輸入。

   `\n` `vbCrLf` Visual Basic) 中的 (代表換行字元。

   字串前面的貨幣符號 (`$`) 可讓您在字串中將運算式（例如變數名稱）放在大括弧中。 運算式值會插入字串以取代運算式。 這個語法稱為 [插補字串](../../csharp/language-reference/tokens/interpolated.md)。

1. 按下<kbd>Ctrl</kbd> + <kbd>F5</kbd>以執行程式，而不進行偵錯工具。

1. 輸入名稱並按 <kbd>enter</kbd> 鍵，以回應提示。

   ![含有已修改程式輸出的主控台視窗](./media/with-visual-studio/hello-world-update.png)

1. 按任意鍵關閉主控台視窗。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已建立 .NET Core 主控台應用程式。 在下一個教學課程中，您將會對應用程式進行偵錯工具。

> [!div class="nextstepaction"]
> [使用 Visual Studio 來對 .NET Core 主控台應用程式進行偵錯工具](debugging-with-visual-studio.md)
