---
title: 作法：使用資料保護
description: 瞭解如何藉由存取 .NET 中的資料保護 API (DPAPI) 來使用資料保護。
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DPAPI
- encryption [.NET], data protection API
- data [.NET], decryption
- ProtectedMemory class, about ProtectedMemory class
- ProtectedData class, about ProtectedData class
- cryptography [.NET], data protection API
- data protection API [.NET]
- decryption
- data [.NET], encryption
ms.assetid: 606698b0-cb1a-42ca-beeb-0bea34205d20
ms.openlocfilehash: 263a07ddf357734e819fffdd41cdff60657adf15
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557056"
---
# <a name="how-to-use-data-protection"></a>作法：使用資料保護

> [!NOTE]
> 本文適用于 Windows。
>
> 如需 ASP.NET Core 的詳細資訊，請參閱[ASP.NET Core 資料保護](/aspnet/core/security/data-protection/introduction)。

.NET 提供資料保護 API (DPAPI) 的存取，可讓您使用來自目前使用者帳戶或電腦的資訊來加密資料。  當您使用 DPAPI 時，會減少明確產生並儲存密碼編譯金鑰的困難問題。  
  
使用 <xref:System.Security.Cryptography.ProtectedData> 類別來加密位元組陣列的複本。 這項功能可在 .NET Framework、.NET Core 和 .NET 5 中取得。  您可以指定由目前使用者帳戶加密的資料只能由相同的使用者帳戶解密，或是您可以指定目前使用者帳戶加密的資料可以由電腦上的任何帳戶解密。  請參閱 <xref:System.Security.Cryptography.DataProtectionScope> 列舉，以取得 <xref:System.Security.Cryptography.ProtectedData> 選項的詳細說明。  
  
### <a name="to-encrypt-data-to-a-file-or-stream-using-data-protection"></a>使用資料保護將資料加密到檔案或資料流  
  
1. 建立隨機的 entropy。  
  
2. 呼叫靜態 <xref:System.Security.Cryptography.ProtectedData.Protect%2A> 方法並傳遞要加密的位元組陣列、entropy 及資料保護範圍。  
  
3. 將加密的資料寫入檔案或資料流。  
  
### <a name="to-decrypt-data-from-a-file-or-stream-using-data-protection"></a>使用資料保護從檔案或資料流解密資料  
  
1. 從檔案或資料流讀取加密的資料。  
  
2. 呼叫靜態 <xref:System.Security.Cryptography.ProtectedData.Unprotect%2A> 方法並傳遞要解密的位元組陣列及資料保護範圍。  
  
## <a name="example"></a>範例

下列程式碼範例示範兩種形式的加密和解密。  首先，程式碼範例會加密，然後再解密記憶體中的位元組陣列。  接下來，程式碼範例會加密位元組陣列的複本、將它儲存至檔案、從檔案載入資料，然後再解密資料。  此範例會顯示原始資料、加密的資料和解密的資料。

[!code-csharp[DPAPI-HowTO#1](../../../samples/snippets/csharp/VS_Snippets_CLR/DPAPI-HowTO/cs/sample.cs#1)]
[!code-vb[DPAPI-HowTO#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DPAPI-HowTO/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>編譯程式碼  

這個範例只會在以 .NET Framework 為目標並在 Windows 上執行時，才會進行編譯和執行

- 包含 `System.Security.dll` 的參考。  
  
- 包含 <xref:System>、<xref:System.IO>、<xref:System.Security.Cryptography> 和 <xref:System.Text> 命名空間。  
  
## <a name="see-also"></a>另請參閱

- [密碼編譯模型](cryptography-model.md)
- [密碼編譯服務](cryptographic-services.md)
- [跨平臺密碼編譯](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.ProtectedMemory>
- <xref:System.Security.Cryptography.ProtectedData>
- [ASP.NET Core 資料保護](/aspnet/core/security/data-protection/introduction)
