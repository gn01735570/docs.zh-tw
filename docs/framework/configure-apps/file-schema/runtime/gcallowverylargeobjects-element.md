---
title: <gcAllowVeryLargeObjects> 項目
ms.date: 03/30/2017
helpviewer_keywords:
- gcAllowVeryLargeObjects element
- <gcAllowVeryLargeObjects> element
ms.assetid: 5c7ea24a-39ac-4e5f-83b7-b9f9a1b556ab
ms.openlocfilehash: 78a42596aae6c3ea0d94ac759d11ed52d0ace539
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178225"
---
# <a name="gcallowverylargeobjects-element"></a>\<gcAllowVeryLargeObjects> 項目

在 64 位元平台上，啟用總大小大於 2 GB 的陣列。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<gcAllowVeryLargeObjects>**  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<gcAllowVeryLargeObjects
   enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>屬性和項目  

 下列章節說明屬性、子元素和父元素。  
  
### <a name="attributes"></a>屬性  
  
|屬性|描述|  
|---------------|-----------------|  
|`enabled`|必要屬性。<br /><br /> 指定是否在64位平臺上啟用大小總計大於 2 GB 的陣列。|  
  
## <a name="enabled-attribute"></a>啟用屬性  
  
|值|描述|  
|-----------|-----------------|  
|`false`|未啟用大小總計大於 2 GB 的陣列。 此為預設值。|  
|`true`|在64位平臺上，已啟用大小總計大於 2 GB 的陣列。|  
  
### <a name="child-elements"></a>子元素  

 無。  
  
### <a name="parent-elements"></a>父項目  
  
|項目|描述|  
|-------------|-----------------|  
|`configuration`|通用語言執行平台和 .NET Framework 應用程式所使用之每個組態檔中的根項目。|  
|`runtime`|包含有關執行階段初始化選項的資訊。|  
  
## <a name="remarks"></a>備註  

 在您的應用程式佈建檔中使用此元素，可啟用大小超過 2 GB 的陣列，但不會變更物件大小或陣列大小的其他限制：  
  
- 陣列中的元素數目上限為 <xref:System.UInt32.MaxValue?displayProperty=nameWithType> 。  
  
- 任何單一維度中的最大索引為 2147483591 (位元組陣列的 0x7FFFFFC7) 和單一位元組結構的陣列，以及適用于其他類型的 2146435071 (0X7FEFFFFF) 。  
  
- 字串和其他非陣列物件的大小上限不變。  
  
> [!CAUTION]
> 啟用這項功能之前，請確定您的應用程式不包含不安全的程式碼，該程式碼會假設所有陣列的大小都小於 2 GB。 例如，使用陣列做為緩衝區的 unsafe 程式碼可能會受到緩衝區溢位的影響，因為假設陣列不會超過 2 GB。  
  
## <a name="example"></a>範例  

 下列範例顯示如何為應用程式啟用這項功能。  
  
```xml  
<configuration>  
  <runtime>  
    <gcAllowVeryLargeObjects enabled="true" />  
  </runtime>  
</configuration>  
```  
  
## <a name="supported-in"></a>支援於

.NET Framework 4.5 和更新版本

## <a name="see-also"></a>另請參閱

- [執行階段設定結構描述](index.md)
- [設定檔架構](../index.md)
