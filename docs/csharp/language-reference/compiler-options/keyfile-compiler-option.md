---
description: -keyfile (C# 編譯器選項)
title: -keyfile (C# 編譯器選項)
ms.date: 07/20/2015
f1_keywords:
- /keyfile
helpviewer_keywords:
- /keyfile compiler option [C#]
- -keyfile compiler option [C#]
- keyfile compiler option [C#]
ms.assetid: 0815f9de-ace4-4e98-b4c6-13c55dea40c2
ms.openlocfilehash: 5af40da18895d47933cb809d710e31a40f14513b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91152425"
---
# <a name="-keyfile-c-compiler-options"></a>-keyfile (C# 編譯器選項)

指定包含密碼編譯金鑰的檔名。  
  
## <a name="syntax"></a>語法  
  
```console  
-keyfile:file  
```  
  
## <a name="arguments"></a>引數  
  
|詞彙|定義|  
|----------|----------------|  
|`file`|包含強式名稱金鑰的檔名。|  
  
## <a name="remarks"></a>備註  

 使用此選項時，編譯器會從指定的檔案將公開金鑰插入資訊清單中，然後使用私密金鑰簽署最終組件。 若要產生金鑰檔，請在命令列鍵入 sn -k `file`。  
  
 如果您使用 **-target:module** 進行編譯，則在使用 [-addmodule](./addmodule-compiler-option.md) 編譯組件時，金鑰檔的名稱會保留在模組中並併入組件。  
  
 您也可以使用 [-keycontainer](./keycontainer-compiler-option.md) 將加密資訊傳遞給編譯器。 如需部分簽署的組件，請使用 [-delaysign](./delaysign-compiler-option.md)。  
  
 如果在相同編譯中同時指定 -keyfile 和 -keycontainer (藉由命令列選項或是自訂屬性指定)，編譯器會先嘗試使用金鑰容器。 如果這個動作成功，則會使用金鑰容器中的資訊來簽署組件。 如果編譯器找不到金鑰容器，則會嘗試使用以 -keyfile 指定的檔案。 如果這個動作成功，則會使用金鑰容器中的資訊來簽署組件，並將金鑰資訊安裝在金鑰容器中 (類似於 sn -i)，這樣在下次編譯時，金鑰容器就會是有效的。  
  
 請注意，金鑰檔可能只包含公開金鑰。  
  
 如需詳細資訊，請參閱[建立和使用強式名稱的組件](../../../standard/assembly/create-use-strong-named.md)和[延遲簽署組件](../../../standard/assembly/delay-sign.md)。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 開發環境中設定這個編譯器選項  
  
1. 開啟專案的 [屬性] **** 頁面。  
  
2. 按一下 [簽署]**** 屬性頁。  
  
3. 修改 [選擇強式名稱金鑰檔]**** 屬性。  
  
 您可以使用 <xref:VSLangProj.ProjectProperties.AssemblyOriginatorKeyFile%2A>，以程式設計方式存取這個編譯器選項。  
  
## <a name="see-also"></a>另請參閱

- [C# 編譯器選項](./index.md)
- [管理專案和方案屬性](/visualstudio/ide/managing-project-and-solution-properties)
