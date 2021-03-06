---
title: System.Object 方法
ms.date: 03/30/2017
ms.assetid: 5397fca0-689e-443e-802f-e1cbdc866427
ms.openlocfilehash: d2b96ca9e7bedd0e0438b47698d4b963844c92f3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155636"
---
# <a name="systemobject-methods"></a>System.Object 方法

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支援下列 <xref:System.Object> 方法。  
  
|||  
|-|-|  
|<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType>|<xref:System.Object.Equals%28System.Object%2CSystem.Object%29?displayProperty=nameWithType>|  
|<xref:System.Object.ToString?displayProperty=nameWithType>||  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支援下列 <xref:System.Object> 方法。  
  
|||  
|-|-|  
|<xref:System.Object.GetHashCode?displayProperty=nameWithType>|<xref:System.Object.ReferenceEquals%28System.Object%2CSystem.Object%29?displayProperty=nameWithType>|  
|<xref:System.Object.MemberwiseClone?displayProperty=nameWithType>|<xref:System.Object.GetType?displayProperty=nameWithType>|  
|二進位型別 (例如 <xref:System.Object.ToString?displayProperty=nameWithType>、`BINARY`、`VARBINARY` 和 `IMAGE`) 的 `TIMESTAMP`。||  
  
## <a name="differences-from-net"></a>與 .NET 的差異  

 Double 的輸出會 <xref:System.Object.ToString?displayProperty=nameWithType> 使用 sql `CONVERT` (NVARCHAR (30) 、 @x ，2) on sql。 在此情況下，SQL 一律會使用 16 位數和科學記號表示法 (例如，"0.000000000000000e+000" 代表 0)。 因此，<xref:System.Object.ToString?displayProperty=nameWithType> 轉換不會產生與 .NET Framework 中 <xref:System.Convert.ToString%2A?displayProperty=nameWithType> 相同的字串。  
  
## <a name="see-also"></a>另請參閱

- [資料類型與函式](data-types-and-functions.md)
