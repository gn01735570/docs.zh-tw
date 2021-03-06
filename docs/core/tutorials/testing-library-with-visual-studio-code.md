---
title: 使用 Visual Studio Code 測試具有 .NET Core 的 .NET Standard 類別庫
description: 建立 .NET Core 類別庫的單元測試專案。 確認 .NET Core 類別庫可在單元測試中正確運作。
ms.date: 06/08/2020
ms.openlocfilehash: 6ae8f6637319cd2c8c24f3e673fb6094f36b9f2f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180448"
---
# <a name="tutorial-test-a-net-standard-class-library-with-net-core-using-visual-studio-code"></a>教學課程：使用 Visual Studio Code 以 .NET Core 測試 .NET Standard 類別庫

本教學課程示範如何將測試專案加入至方案，以自動化單元測試。

## <a name="prerequisites"></a>必要條件

- 本教學課程適用于 [使用 Visual Studio Code 建立 .NET Standard 程式庫](library-with-visual-studio-code.md)中所建立的解決方案。

## <a name="create-a-unit-test-project"></a>建立單元測試專案

單元測試能在開發與發佈期間提供自動化的軟體測試。 您在本教學課程中使用的測試架構是 MSTest。 [MSTest](https://github.com/Microsoft/testfx-docs) 是您可以從中選擇的三種測試架構之一。 其他則為 [xUnit](https://xunit.net/) 和 [nUnit](https://nunit.org/)。

1. 啟動 Visual Studio Code。

1. 開啟 `ClassLibraryProjects` 您在 [使用 Visual Studio Code 建立 .NET Standard 程式庫](library-with-visual-studio-code.md)中建立的方案。

1. 建立名為 "StringLibraryTest" 的單元測試專案。

   ```dotnetcli
   dotnet new mstest -o StringLibraryTest
   ```

   專案範本會使用下列程式碼建立 UnitTest1.cs 檔案：

   ```csharp
   using Microsoft.VisualStudio.TestTools.UnitTesting;

   namespace StringLibraryTest
   {
       [TestClass]
       public class UnitTest1
       {
           [TestMethod]
           public void TestMethod1()
           {
           }
       }
   }
   ```

   單元測試範本建立的原始程式碼會執行下列動作︰

   - 它會匯入 <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType> 命名空間，其中包含用於單元測試的類型。
   - 它會將 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 屬性套用至 `UnitTest1` 類別。
   - 它會套用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 屬性來定義 `TestMethod1` 。

   執行單元測試時，會自動執行在標記為[[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute)的測試類別中，以[[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)標記的每個方法。

1. 將測試專案加入至方案。

   ```dotnetcli
   dotnet sln add StringLibraryTest/StringLibraryTest.csproj
   ```

## <a name="add-a-project-reference"></a>新增專案參考

若要讓測試專案使用 `StringLibrary` 類別，請在專案中加入專案的參考 `StringLibraryTest` `StringLibrary` 。

1. 執行以下命令：

   ```dotnetcli
   dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj
   ```

## <a name="add-and-run-unit-test-methods"></a>加入並執行單元測試方法

當 Visual Studio 執行單元測試時，它會執行每個以屬性標記之 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 類別中的屬性標記的方法  <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 。 當找到第一個失敗或方法中包含的所有測試都成功時，測試方法便會結束。

最常見的測試會呼叫 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 類別的成員。 許多判斷提示方法都至少包括兩個參數，一個是預期的測試結果，另一個是實際的測試結果。 `Assert`下表顯示某些類別最常呼叫的方法：

| Assert 方法     | 函式 |
| ------------------ | -------- |
| `Assert.AreEqual`  | 驗證兩個值或物件相等。 如果值或物件不相等，判斷提示就會失敗。 |
| `Assert.AreSame`   | 驗證兩個物件變數參考相同的物件。 如果變數參考不同的物件，判斷提示就會失敗。 |
| `Assert.IsFalse`   | 驗證條件為 `false`。 如果條件為 `true`，判斷提示就會失敗。 |
| `Assert.IsNotNull` | 確認物件不是 `null` 。 如果物件為 `null`，判斷提示就會失敗。 |

您也可以 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.ThrowsException%2A?displayProperty=nameWithType> 在測試方法中使用方法，來指出預期會擲回的例外狀況類型。 如果未擲回指定的例外狀況，測試便會失敗。

在測試 `StringLibrary.StartsWithUpper` 方法時，您想提供幾個以大寫字元開頭的字串。 您預期此方法在這些情況下傳回 `true`，因此您可以呼叫 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsTrue%2A?displayProperty=nameWithType> 方法。 同樣地，您想提供幾個開頭不是大寫字元的字串。 您預期此方法在這些情況下傳回 `false`，因此您可以呼叫 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsFalse%2A?displayProperty=nameWithType> 方法。

因為您的程式庫方法會處理字串，您也想要確保它能成功處理 [空字串 (`String.Empty`) ](xref:System.String.Empty) 和和 `null` 字串。 空字串是指沒有任何字元且其 <xref:System.String.Length> 為0的字串。 `null`字串是指尚未初始化的字串。 您可以 `StartsWithUpper` 直接呼叫作為靜態方法，並傳遞單一 <xref:System.String> 引數。 或者，您可以 `StartsWithUpper` 在指派給的變數上呼叫做為擴充方法 `string` `null` 。

您會定義三個方法，每個方法會 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 針對字串陣列中的每個元素呼叫方法。 您將呼叫方法多載，讓您指定在測試失敗時所要顯示的錯誤訊息。 訊息會識別造成失敗的字串。

建立測試方法：

1. 開啟 *StringLibraryTest/UnitTest1* ，並將所有程式碼取代為下列程式碼。

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibraryTest/UnitTest1.cs":::

   方法中的大寫字元測試 `TestStartsWithUpper` 包含希臘文大寫字母 Alpha (u + 0391) 和斯拉夫文大寫字母 EM (U + 041C) 。 方法中的小寫字元測試 `TestDoesNotStartWithUpper` 包含希臘文小寫字母 Alpha (u + 03B1) 和斯拉夫文小寫字母 Ghe (U + 0433) 。

1. 儲存變更。

1. 執行測試：

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   終端機輸出顯示所有測試皆通過。

   ```output
   Starting test execution, please wait...

   A total of 1 test files matched the specified pattern.

   Test Run Successful.
   Total tests: 3
        Passed: 3
    Total time: 5.1116 Seconds
   ```

## <a name="handle-test-failures"></a>處理測試失敗

如果您正在執行以測試為導向的開發 (TDD) ，您會先撰寫測試，並在您第一次執行時失敗。 然後，將程式碼新增至應用程式，讓測試成功。 在本教學課程中，您已在撰寫應用程式程式碼驗證之後建立測試，因此您未看到測試失敗。 若要在預期測試失敗時驗證測試失敗，請將不正確值加入測試輸入。

1. 修改 `TestDoesNotStartWithUpper` 方法中的 `words` 陣列，以包含字串「錯誤」。

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

1. 執行測試：

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   終端機輸出顯示一個測試失敗，並針對失敗的測試提供錯誤訊息：「IsFalse 失敗。 'Error' 的預期︰false；實際：True」。 因為失敗，所以在測試 "Error" 之後，陣列中沒有任何字串。

   ```output
   Starting test execution, please wait...

   A total of 1 test files matched the specified pattern.
     X TestDoesNotStartWithUpper [283ms]
     Error Message:
      Assert.IsFalse failed. Expected for 'Error': false; Actual: True
     Stack Trace:
        at StringLibraryTest.UnitTest1.TestDoesNotStartWithUpper() in C:\
   Projects\ClassLibraryProjects\StringLibraryTest\UnitTest1.cs:line 33

   Test Run Failed.
   Total tests: 3
        Passed: 2
        Failed: 1
    Total time: 1.7825 Seconds
   ```

1. 移除您在步驟1中新增的字串 "Error"。 重新執行測試和測試階段。

## <a name="test-the-release-version-of-the-library"></a>測試程式庫的發行版本

現在，在執行程式庫的偵錯工具時，所有測試都已通過，請針對程式庫的發行組建執行額外的測試。 許多因素 (包括編譯器最佳化) 有時會在偵錯和發行組建之間導致不同的行為。

1. 使用發行組建設定執行測試：

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj --configuration Release
   ```

   所有測試皆通過。

## <a name="debug-tests"></a>偵錯測試

如果您使用 Visual Studio Code 作為 IDE，您可以使用在使用您的單元測試專案來對程式碼進行偵錯工具時，使用 Visual Studio Code 來將 [.Net Core 主控台應用程式的偵錯工具](debugging-with-visual-studio-code.md) 中所顯示的相同程式。 開啟*StringLibraryTest/UnitTest1*，然後選取 [執行行7和8之間的**所有測試**]，而不是啟動*展示*應用程式專案。 如果找不到，請按<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>開啟命令選擇區，然後輸入**重載視窗**。

Visual Studio Code 啟動已附加偵錯工具的測試專案。 執行將會在您已新增至測試專案或基礎程式庫程式碼的任何中斷點停止執行。

## <a name="additional-resources"></a>其他資源

* [.NET Core 與 .NET Standard 中的單元測試](../testing/index.md)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已對類別庫進行單元測試。 您可以將程式庫以套件的形式發佈至 [NuGet](https://nuget.org) ，以將程式庫提供給其他人使用。 若要深入瞭解，請遵循 NuGet 教學課程：

> [!div class="nextstepaction"]
> [使用 dotnet CLI 建立及發佈套件](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

如果您將程式庫發佈為 NuGet 套件，其他人可以安裝和使用它。 若要深入瞭解，請遵循 NuGet 教學課程：

> [!div class="nextstepaction"]
> [利用 dotnet CLI 安裝並使用套件](/nuget/quickstart/install-and-use-a-package-using-the-dotnet-cli)

程式庫不需要以套件的形式散發。 它可以與使用它的主控台應用程式配套。 若要瞭解如何發佈主控台應用程式，請參閱本系列的先前教學課程：

> [!div class="nextstepaction"]
> [使用 Visual Studio Code 發佈 .NET Core 主控台應用程式](publishing-with-visual-studio-code.md)
