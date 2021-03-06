---
title: ICorProfilerCallback::ClassLoadStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ClassLoadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ClassLoadStarted
helpviewer_keywords:
- ICorProfilerCallback::ClassLoadStarted method [.NET Framework profiling]
- ClassLoadStarted method [.NET Framework profiling]
ms.assetid: 9f728de8-45c2-45a5-ac4a-45660bd36ecf
topic_type:
- apiref
ms.openlocfilehash: 9a9fdc80c8f63dd5b004953266a5d7399655bc71
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500360"
---
# <a name="icorprofilercallbackclassloadstarted-method"></a>ICorProfilerCallback::ClassLoadStarted 方法
通知分析工具，正在載入類別。  
  
## <a name="syntax"></a>語法  
  
```cpp  
HRESULT ClassLoadStarted(  
    [in] ClassID classId);  
```  
  
## <a name="parameters"></a>參數

- `classId`

  \[在中，識別要載入的類別。

## <a name="remarks"></a>備註  
 在 `classId` 呼叫[ICorProfilerCallback：： ClassLoadFinished](icorprofilercallback-classloadfinished-method.md)方法之前，的值對資訊要求而言是不正確。  
  
## <a name="requirements"></a>規格需求  
 **平台：** 請參閱[系統需求](../../get-started/system-requirements.md)。  
  
 **標頭：** CorProf.idl、CorProf.h  
  
 **程式庫：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另請參閱

- [ICorProfilerCallback 介面](icorprofilercallback-interface.md)
