---
title: dotnet remove package 命令
description: dotnet remove package 命令提供方便的選項，以移除專案的 NuGet 套件參考。
ms.date: 02/14/2020
ms.openlocfilehash: fc74ac1364a0ed027b83dab270d382f238dc00e5
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463449"
---
# <a name="dotnet-remove-package"></a>dotnet remove package

**本文適用於:✔️** .NET Core 2.x SDK 和更高版本

## <a name="name"></a>名稱

`dotnet remove package` - 從專案檔中移除套件參考。

## <a name="synopsis"></a>概要

```dotnetcli
dotnet remove [<PROJECT>] package <PACKAGE_NAME>

dotnet remove package -h|--help
```

## <a name="description"></a>描述

`dotnet remove package` 命令提供方便的選項，以從專案中移除 NuGet 套件參考。

## <a name="arguments"></a>引數

`PROJECT`

指定專案檔。 如果未指定，命令會在目前的目錄中搜尋一個專案檔。

`PACKAGE_NAME`

要移除的套件參考。

## <a name="options"></a>選項。

- **`-h|--help`**

  印出命令的簡短說明。

## <a name="examples"></a>範例

- 從`Newtonsoft.Json`目前目錄中移除 NuGet 套件:

  ```dotnetcli
  dotnet remove package Newtonsoft.Json
  ```
