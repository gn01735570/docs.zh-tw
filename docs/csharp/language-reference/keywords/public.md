---
description: public 關鍵字 - C# 參考
title: public 關鍵字 - C# 參考
ms.date: 07/20/2015
f1_keywords:
- public
- public_CSharpKeyword
helpviewer_keywords:
- public keyword [C#]
ms.assetid: 0ae45d16-a551-4b74-9845-57208de1328e
ms.openlocfilehash: 26edaf7538d11d082a4b8863228213c3ebc46937
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2020
ms.locfileid: "89122338"
---
# <a name="public-c-reference"></a>public (C# 參考)

`public` 關鍵字是類型和類型成員的存取修飾詞。 公用存取是最寬鬆的存取層級。 不會限制存取公用成員，如此範例所示︰

```csharp
class SampleClass
{
    public int x; // No access restrictions.
}
```

如需詳細資訊，請參閱[存取修飾詞](../../programming-guide/classes-and-structs/access-modifiers.md)和[存取範圍層級](accessibility-levels.md)。

## <a name="example"></a>範例

在下列範例中，宣告兩個類別：`PointTest` 和 `MainClass`。 `PointTest`的公用成員 `x` 和 `y` 直接存取自 `MainClass`。

[!code-csharp[csrefKeywordsModifiers#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#13)]

如果您將 `public` 存取層級變更為 [private](private.md) 或 [protected](protected.md)，則會收到錯誤訊息：

'PointTest.y' 的保護層級導致無法對其進行存取。

## <a name="c-language-specification"></a>C# 語言規格  

如需詳細資訊，請參閱 [C# 語言規格](/dotnet/csharp/language-reference/language-specification/introduction)的[已宣告存取範圍](~/_csharplang/spec/basic-concepts.md#declared-accessibility)。 語言規格是 C# 語法及用法的限定來源。

## <a name="see-also"></a>另請參閱

- [C # 參考](../index.md)
- [C # 程式設計指南](../../programming-guide/index.md)
- [存取修飾詞](../../programming-guide/classes-and-structs/access-modifiers.md)
- [C # 關鍵字](index.md)
- [存取修飾詞](access-modifiers.md)
- [協助工具層級](accessibility-levels.md)
- [修飾詞](index.md)
- [私人](private.md)
- [protected](protected.md)
- [internal](internal.md)
