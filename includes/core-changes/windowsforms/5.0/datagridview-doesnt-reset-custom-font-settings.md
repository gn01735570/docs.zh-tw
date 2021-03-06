---
ms.openlocfilehash: 9c84c9f844a2a7efb9633c11634b069ef6b6033b
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2020
ms.locfileid: "91804854"
---
### <a name="datagridview-no-longer-resets-fonts-for-customized-cell-styles"></a>DataGridView 不會再重設自訂儲存格樣式的字型

當環境字型變更時， <xref:System.Windows.Forms.DataGridViewElement.DataGridView> 如果資料格樣式字型已自訂，就不會再重設預設儲存格樣式字型以符合環境字型。

#### <a name="change-description"></a>變更描述

在舊版的 .NET 版本中，如果環境字型變更，會 <xref:System.Windows.Forms.DataGridViewElement.DataGridView> 重設和覆寫、和屬性中的使用者定義字型 <xref:System.Windows.Forms.DataGridView.DefaultCellStyle> <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle> 。

從 .NET 5.0 開始，如果您在、或屬性中設定字型設定 <xref:System.Windows.Forms.DataGridView.DefaultCellStyle> <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle> ，即使環境字型變更，這些設定仍會保留。 針對任何您未自訂字型的屬性，字型將會變更以符合環境字型設定。

#### <a name="reason-for-change"></a>變更的原因

當 [.Net Core 3.0 中的預設字型變更](../../../../docs/core/compatibility/winforms.md#default-control-font-changed-to-segoe-ui-9-pt)時，各種儲存格樣式的預設字型設定也會變更。 如果應用程式依賴其控制項中的自訂樣式 <xref:System.Windows.Forms.DataGridViewElement.DataGridView> ，並阻礙將這些應用程式從 .NET Framework 遷移至 .net 5.0，則不需要這種行為。

#### <a name="version-introduced"></a>引進的版本

.NET 5.0 RC2

#### <a name="recommended-action"></a>建議的動作

在您的部分上不需要採取任何動作。 但是，如果您已自訂、或屬性中的字型， <xref:System.Windows.Forms.DataGridView.DefaultCellStyle> <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle> <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle> 並希望字型符合環境字型，請 <xref:System.Windows.Forms.DataGridViewCellStyle.Font?displayProperty=nameWithType> `null` 針對每個屬性設定為。

#### <a name="category"></a>類別

- Windows Forms

#### <a name="affected-apis"></a>受影響的 API

- <xref:System.Windows.Forms.DataGridView.DefaultCellStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle?displayProperty=fullName>

<!--

#### Affected APIs

- `P:System.Windows.Forms.DataGridView.DefaultCellStyle`
- `P:System.Windows.Forms.DataGridView.ColumnHeadersDefaultCellStyle`
- `P:System.Windows.Forms.DataGridView.RowHeadersDefaultCellStyle`

-->
