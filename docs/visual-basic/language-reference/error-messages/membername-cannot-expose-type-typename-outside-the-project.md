---
title: "'<membername>' 無法透過 <typename> '<containertype>' 在專案外部公開類型 '<containertypename>'"
ms.date: 07/20/2015
f1_keywords:
- bc30909
- vbc30909
helpviewer_keywords:
- BC30909
ms.assetid: ffa7395d-e182-4087-8ce8-079810fdae54
ms.openlocfilehash: a3972eabfe297b89c0e4d0f36943ac58e5bdf688
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162462"
---
# <a name="bc30909-membername-cannot-expose-type-typename-outside-the-project-through-containertype-containertypename"></a>BC30909： ' \<membername> ' 無法 \<typename> 透過 \<containertype> ' ' 在專案外部公開類型 ' \<containertypename> '

變數、程式參數或函式傳回會在其容器之外公開，但會宣告為不能在容器外部公開的類型。

 下列基本架構程式碼顯示會產生此錯誤的情況。

```vb
Private Class privateClass
End Class
Public Class mainClass
    Public exposedVar As New privateClass
End Class
```

 宣告為、、或的型別， `Protected` `Friend` `Protected Friend` `Private` 目的是要在其宣告內容之外擁有有限的存取權。 將它當作限制存取權的變數資料類型，就會破壞這個目的。 在上述的基本架構程式碼中， `exposedVar` `Public` 和會公開 `privateClass` 至不應該有其存取權的程式碼。

 **錯誤識別碼：** BC30909

## <a name="to-correct-this-error"></a>更正這個錯誤

- 變更變數、程式參數或函式的存取層級，至少要與其資料類型的存取層級限制相同。

## <a name="see-also"></a>另請參閱

- [Visual Basic 中的存取層級](../../programming-guide/language-features/declared-elements/access-levels.md)
