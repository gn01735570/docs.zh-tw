---
title: 類型 '<typename>' 沒有建構函式
ms.date: 07/20/2015
f1_keywords:
- bc30251
- vbc30251
helpviewer_keywords:
- BC30251
ms.assetid: aff3e1df-abe6-4bc0-9abc-a1e70514c561
ms.openlocfilehash: 249bcb7020f26c7c43d560e91ef7a34e4dc64470
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161175"
---
# <a name="bc30251-type-typename-has-no-constructors"></a>BC30251：類型 ' \<typename> ' 沒有任何函數

類型不支援呼叫 `Sub New()`。 一個可能原因是編譯器或二進位檔案損毀。

 **錯誤識別碼：** BC30251

## <a name="to-correct-this-error"></a>更正這個錯誤

1. 如果類型是在不同專案中，或是在參考的檔案中，請重新安裝專案或檔案。

2. 如果類型是在相同專案中，請重新編譯包含類型的組件。

3. 如果錯誤重複發生，請重新安裝 Visual Basic 編譯器。

4. 如果錯誤持續發生，請收集情況的相關資訊，並通知 Microsoft 產品支援服務。

## <a name="see-also"></a>另請參閱

- [物件和類別](../../programming-guide/language-features/objects-and-classes/index.md)
- [與我們交談](/visualstudio/ide/feedback-options)
