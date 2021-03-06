---
title: ICLRIoCompletionManager 介面
ms.date: 03/30/2017
api_name:
- ICLRIoCompletionManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRIoCompletionManager
helpviewer_keywords:
- ICLRIoCompletionManager interface [.NET Framework hosting]
ms.assetid: c6c3ace6-e5e7-4450-8cc5-a9a48208c493
topic_type:
- apiref
ms.openlocfilehash: 71afc5e9772f82b922e8f428e6d808e46d092704
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2020
ms.locfileid: "84504208"
---
# <a name="iclriocompletionmanager-interface"></a>ICLRIoCompletionManager 介面
會執行回呼方法，讓主機能夠通知 common language runtime （CLR）所指定 i/o 要求的狀態。  
  
## <a name="methods"></a>方法  
  
|方法|描述|  
|------------|-----------------|  
|[OnComplete 方法](iclriocompletionmanager-oncomplete-method.md)|通知 CLR，這是使用[IHostIoCompletionManager：： Bind](ihostiocompletionmanager-bind-method.md)方法呼叫所提出的 i/o 要求狀態。|  
  
## <a name="remarks"></a>備註  
 主機會使用[IHostIoCompletionManager](ihostiocompletionmanager-interface.md)介面來執行 i/o 完成抽象概念。 CLR 會透過這個介面提出 i/o 要求，而主機會使用介面來通知執行時間此類要求的結果 `ICLRIoCompletionManager` 。  
  
## <a name="requirements"></a>規格需求  
 **平台：** 請參閱[系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** Mscoree.dll. h  
  
 連結**庫：** 包含為 Mscoree.dll 中的資源  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [IHostIoCompletionManager 介面](ihostiocompletionmanager-interface.md)
- [IHostThreadPoolManager 介面](ihostthreadpoolmanager-interface.md)
- [裝載介面](hosting-interfaces.md)
