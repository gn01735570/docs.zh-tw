---
description: -recurse (C# 編譯器選項)
title: -recurse (C# 編譯器選項)
ms.date: 07/20/2015
f1_keywords:
- /recurse
helpviewer_keywords:
- /recurse compiler option [C#]
- recurse compiler option [C#]
- -recurse compiler option [C#]
ms.assetid: 4e8212e5-04e3-45b1-8a42-41bc50e683b0
ms.openlocfilehash: 9e84ff95f7f0addac1c2c2d79af0ab53572da27f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91193799"
---
# <a name="-recurse-c-compiler-options"></a>-recurse (C# 編譯器選項)

-recurse 選項可讓您編譯指定目錄 (dir) 或專案目錄之所有子目錄中的原始程式碼檔。  
  
## <a name="syntax"></a>語法  
  
```console  
-recurse:[dir\]file  
```  
  
## <a name="arguments"></a>引數  

 `dir` (選擇性)  
 您想要開始搜尋的目錄。 如果未指定此項目，則會在專案目錄中開始搜尋。  
  
 `file`  
 要搜尋的檔案。 允許萬用字元。  
  
## <a name="remarks"></a>備註  

 **-遞迴**選項可讓您在指定的目錄 (`dir`) 或專案目錄的所有子目錄中，編譯原始程式碼檔。  
  
 您可以在檔案名中使用萬用字元，來編譯專案目錄中的所有相符檔案，而不需要使用 **-遞迴**。  
  
 Visual Studio 不提供這個編譯器選項，您亦無法以程式設計方式變更。  
  
## <a name="example"></a>範例  

 編譯目前目錄中的所有 C# 檔案：  
  
```console  
csc *.cs  
```  
  
 編譯 dir1\dir2 目錄和其下任何目錄中的所有 C# 檔案，並產生 dir2.dll：  
  
```console  
csc -target:library -out:dir2.dll -recurse:dir1\dir2\*.cs  
```  
  
## <a name="see-also"></a>另請參閱

- [C# 編譯器選項](./index.md)
- [管理專案和方案屬性](/visualstudio/ide/managing-project-and-solution-properties)
